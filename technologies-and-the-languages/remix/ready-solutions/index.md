# Готовые решения

## CreateCookieSessionStorage

Создание сессии, значение которой будет храниться в захешированном виде в cookie файлах.
Помимо этого, результатом работы функции являются интерфейсы для работы с данными сессии.

```ts
const { getSession, commitSession, destroySession } =
  createCookieSessionStorage({
    cookie: {
      name: "__session",
      secure: process.env.NODE_ENV === "production",
      sameSite: "lax",
      maxAge: SESSION_MAX_AGE, // время в секундах
      path: "/",
      expires: new Date(Date.now() + SESSION_MAX_AGE),
      httpOnly: true,
      secrets: [process.env.SECRET_SESSION_KEY!],
    },
  });

export { getSession, commitSession, destroySession };
```

## Защита роутинга от не авторизованных пользователя.

Вся обработка запросов на сервер проходит через Remix API. Для этого, в файлах, расположенных в папке routes добавляются
новые функции - [action](https://remix.run/docs/en/v1/api/conventions#action) и [loader](https://remix.run/docs/en/v1/api/conventions#loader).
Remix не является сторонником middleware (и это хорошо), а поэтому мы должны сами задумываться о защите маршрутов роутера
от не авторизованных пользователей. В связи с этим, в каждом routes файле (кроме login), в функциях loader первой строчкой вызывается
функция `requireUserSession`. Помимо проверки наличия сессии авторизации пользователя, в случае наличия этой сессии она возвращает
её, как результат работы функции. Поэтому если вам понадобятся данные пользователя, как например в root.tsx для навигационного меню,
вы можете получить из неё необходимые данные.

```ts
export async function requireUserSession(request: Request) {
  const cookie = request.headers.get(HEADERS_COOKIE);
  const session = await getSession(cookie);
  const redirectUrl = urlManager.getLoginUrl();

  if (!session.has(SESSION_USER_DATA)) {
    throw redirect(redirectUrl);
  }

  return session;
}
```

## Кастомный fetch

Remix использует WEB fetch API. При этом добавляя определённую магию (создание полифила), чтобы данный инструмент
мог работать с action и loader функциями, которые [исполняются на сервере](https://remix.run/docs/en/v1/guides/constraints).
Отсылаясь к предыдущему пункту "Границы ошибок", важно отметить один нюанс. В стандартной документации по [fetch API](https://developer.mozilla.org/ru/docs/Web/API/Fetch_API)
сказано, что ответ со статусом 400+ и 500+ не является для fetch исключением. Вместо этого генерируется JSON ответ с кодом ошибки.
Поэтому необходимо проверять статус ok, чтобы выявить проблемы. Пример:

```js
const getFisrtTodo = async () => {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/todos/1"
    );

    if (!response.ok) {
      throw new Error(" Ошибка :( ");
    }

    const data = await response.json();

    return data;
  } catch (error) {
    console.log(error);
  }
};
```

1. Пожалуйста, не делайте так! Полифил fetch, предоставляющийся Remix, содержит в себе встроенную обработку исключений. Это значит, что fetch сам проверит статус ответа (response.ok) и в случае чего выбросит ошибку.

2. Не используйте try catch блоки. При таком подходе Remix не сможет поймать ошибку, при помощи описанного выше ErrorBoundary и CatchBoundary API. Если вы всё-таки хотите поймать эту ошибку, с помощью try catch блока (например, для того чтобы отправить автоматический отчёт), то убедитесь, что она не останется в нём и дойдёт до границы ошибок Remix API. Пример как это можно сделать:

```js
const getFisrtTodo = async () => {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/todos/1"
    );

    if (!response.ok) {
      throw new Error(" Ошибка брат :( ");
    }

    const data = await response.json();

    return data;
  } catch (error) {
    // ... логика связанная с обработкой
    // ...
    // ...
    throw new Error(error);
  }
};
```

3. Вместо обычного fetch для асинхронных запросов мы используем `fetchWithAuth`. Данная обёртка позволяет нам подставлять необходимый для правильной работы серверной части заголовок со значением токена авторизации.

```ts
export async function fetchWithAuth(
  url: RequestInfo,
  request: Request,
  options?: RequestInit
) {
  const session = await getSession(request.headers.get(HEADERS_COOKIE));
  const authorizationHeaders: HeadersInit = new Headers(options?.headers);

  const tokenAccessValue = session.get(SESSION_TOKEN_ACCESS);

  if (tokenAccessValue) {
    authorizationHeaders.append(
      HEADERS_AUTHORIZATION,
      `Bearer ${tokenAccessValue}`
    );
  }

  return fetch(url, {
    ...options,
    headers: authorizationHeaders,
  });
}
```
