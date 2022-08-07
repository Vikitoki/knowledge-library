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

## RequireUserSession

Функция, предоставляющая работу к сессии и "защищающая" роутинг от пользователей не прошедших авторизацию.

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

## FetchWithAuth

Fetch обёртка, помогающая автоматически подставлять заголовок и токен авторизации, если они доступны.

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
