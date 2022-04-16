# Custom hooks

## Сортировка и поиск элементов

Обратите внимание на useMemo. 
На клиенте очень часто вместе с сортировкой присутствует фильтрация, привязанная к поисковой строке. 
Изменение значения поисковой строки может вызывать постоянный вызов функции сортировки. 
Такое изменение может оказать огромный удар по производительности. 
Если вы уверены, что в вашем случае этого не происходит, пожалуйста удалите useMemo.

```ts
import { useMemo } from "react";

export const useSortedElements = (array: any[], sort: string) => {
  const sortedElements = useMemo(() => {
    if (sort) {
      return [...array].sort((a, b) => a[sort].localeCompare(b[sort]));
    }
    return posts;
  }, [dependencies]);

  return sortedElements;
};

export const useSortedAndSearchedElements = (
  array: any[],
  sort: string,
  query: string
) => {
  const sortedElements = useSortedPosts(array, sort);

  const sortedAndSearchedElements = useMemo(() => {
    return sortedElements.filter((element) =>
      element.title.toLowerCase().includes(query.toLowerCase())
    );
  }, [dependencies]);

  return sortedAndSearchedElements;
};
```

---

## Получение дополнительных состояний во время запрос данных

Внимание, существует определённое соглашение, для корректной работы данного хука:
вы должны предоставить обработку исключений блоку try - catch, который расположен внутри данного хука.
Если вы совершаете обработку где-то глубже в методах, которые используете, пожалуйста после обратки ошибки пробросьте её наверх.

```javascript
import { useState } from "react/index";

export const useFetching = (callback) => {
  const [isLoading, setIsLoading] = useState(false);
  const [isError, setIsError] = useState("");

  const fetching = async (...args) => {
    try {
      setIsLoading(true);
      await callback(...args);
    } catch (error) {
      setError(error.message);
    } finally {
      setIsLoading(false);
    }
  };

  return [fetching, isLoading, isError];
};
```

---

## Выполнение функции после пересечения конкретного блока на странице. 

Технология, использующаяся в данном хуке: <https://developer.mozilla.org/ru/docs/Web/API/Intersection_Observer_API>

- canLoad - флаг, ограничивающий вызов функции.

- isLoading - флаг загрузки, изменяющий своё значение во время запроса на сервер.

- ref - элемент DOM, находящийся на странице элемент, за которым мы следим.

```javascript
import {useEffect, useRef} from "react";

export const useObserver = (ref, canLoad, isLoading, callback) => {
    const observer = useRef();

    useEffect(() => {
        if(isLoading) return;
        if(observer.current) observer.current.disconnect();

        const cb = function(entries, observer) {
            if (entries[0].isIntersecting && canLoad) {
                callback()
            }
        };
        observer.current = new IntersectionObserver(cb);
        observer.current.observe(ref.current)
    }, [isLoading])
}
```

---

## Debounce для функций

Статья по теме: 
<https://medium.com/nuances-of-programming/%D1%87%D1%82%D0%BE-%D1%82%D0%B0%D0%BA%D0%BE%D0%B5-throttling-%D0%B8-debouncing-4f0a839769ef>

- cleanUp - флаг, отвечающий за выполнение функции, в соответствии с размонтированием компонента. 
Если ваш компонент может быть размонтирован, установите флаг в значение true.

```javascript
import { useRef, useEffect } from "react";

export default function useDebouncedFunction(func, delay, cleanUp = false) {
  const timeoutRef = useRef();

  function clearTimer() {
    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
      timeoutRef.current = undefined;
    }
  }

  useEffect(() => (cleanUp ? clearTimer : undefined), [cleanUp]);

  return (...args) => {
    clearTimer();
    timeoutRef.current = setTimeout(() => func(...args), delay);
  };
}
```

---

## Получить предыдущие пропсы или состояние

```jsx
function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  });
  
  return ref.current;
}
```

## Измерить параметры DOM узла

Один из элементарных способов определения положения или размера DOM-узла это использование колбэк-реф.
React будет вызывать этот колбэк всякий раз, когда реф привязывается к другому узлу. 

Если необходимо отследить изменение размера компонента, 
воспользуйтесь [ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) или сторонним хуком, 
использующего этот API.

Пример:

---

```jsx
function useClientRect() {
  const [rect, setRect] = useState(null);
  const ref = useCallback(node => {
    if (node !== null) {
      setRect(node.getBoundingClientRect());
    }
  }, []);
  return [rect, ref];
}
```
