## **Хуки**

### Сортировка и поиск элементов

Здесь добавлена реализация useMemo, так как на фронтенде очень часто вместе с сортировкой, на странице присутствует поиск. Изменение состояния содержиомого поиска может вызывать постоянный вызов функции сортировки, что может оказать огромный удар по производительности. Если вы уверены, что в вашем случае этого не происходит, пожалуйста удалите useMemo.

```javascript
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

export const useSortedAndSearchedElements = (array: any[], sort: string, query: string) => {
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

### Получение дополнительных состояний во время запрос данных

Внимание, важно предоставлять обработку ошибок именно хуку! Это значит, что если вы, например , описываете метод класса в котором заложена основная функциональность по получению данных со стороннего API, пожалуйста , не пишите обработку ошибок (блок try - catch) в этом методе.

```javascript
import {useState} from "react";

export const useFetching = (callback) => {
    const [isLoading, setIsLoading] = useState(false);
    const [isError, setIsError] = useState('');

    const fetching = async (...args) => {
        try {
            setIsLoading(true)
            await callback(...args)
        } catch (error) {
            setError(error.message);
        } finally {
            setIsLoading(false)
        }
    }

    return [fetching, isLoading, isError]
}
```

---

### Отложенная загрузка изображений или другого контента по мере прокрутки страницы. Реализация веб-сайтов с "бесконечным скроллом", где контент подгружается по мере того как страница прокручивается вниз, 

Технология, логика которой используется в данном хуке https://developer.mozilla.org/ru/docs/Web/API/Intersection_Observer_API


```javascript
import {useEffect, useRef} from "react";

 - canLoad - ограничивает вызов функции. Как правило, это boolean значение, которая сравнивает между собой, например, номер текущей страницы и последней.

 - isLoading - состояние загрузки, совершаемое во время запроса на сервер. 

 - ref - находящийся на странице элемент, за которым мы следим.

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