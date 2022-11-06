# JavaScript

1. В чём заключаются различия var, const и let? <https://doka.guide/js/var-let/>
2. Какие типы данных есть в JavaScript? <https://learn.javascript.ru/types>
3. Какие из типов данных являются ссылочными типами в JavaScript? <https://learn.javascript.ru/object-reference>
4. Тип данных symbol: что такое, когда и для чего используется? <https://doka.guide/js/symbol/>
5. Генераторы/итераторы. <https://learn.javascript.ru/generators>
6. Как работает наследование в JS? <https://learn.javascript.ru/class-inheritance>
7. Прототипная модель. Что это? Как организуется родительская цепочка?
   <https://learn.javascript.ru/class-inheritance>
8. Чем отличается **proto** от prototype? <https://learn.javascript.ru/new-prototype>
9. Как узнать обладает ли объект собственным свойством или оно находится в цепочке прототипов?
   <https://learn.javascript.ru/prototype-inheritance>
10. Сколько может быть прототипов у объекта? Какой существует подход для
    реализации двух прототипов? <https://learn.javascript.ru/prototype>
11. Конструкция instanceof. Когда может быть полезна? <https://learn.javascript.ru/instanceof>
12. Разница между == и === ? <https://doka.guide/js/typecasting/>
13. Как изменить свойство объекта возвращая новый объект?
    <https://learn.javascript.ru/property-descriptors>
14. Скольки поточный язык JavaScript?

    JavaScript – язык программирования, который изначально задумывался как одно поточный.
    Это значит, что в одном и том же процессе обрабатывается единственный имеющийся набор инструкций.
    Такой подход можно использовать для облегчения задач, поставленных перед разработчиками.
    При помощи специальных приемов и движков можно реализовать асинхронность.
    В этом случае система возвращает значение не одной функции кода, а нескольких при непосредственной обработке.

15. Как реализована и работает асинхронность?

    Обзорные статьи по теме:

    <https://doka.guide/js/async-in-js/> <br>
    <https://habr.com/ru/company/wrike/blog/302896/> <br>
    <https://habr.com/ru/post/462355/>

    Ссылка на цикл из 19 статей, для более углублённого познания:

    <https://habr.com/ru/company/ruvds/blog/337042/>

16. Ассинхронность и многопоточность. В чём отличия?
    <https://habr.com/ru/company/wrike/blog/302896/>
17. Что такое promise? Когда и для чего используется?
    <https://doka.guide/js/promise/>
18. Паттерны проектирования в JavaScript. <br>
    <https://habr.com/ru/company/ruvds/blog/427293/> <br>
    <https://doka.guide/js/programming-paradigms/>
19. Какие типы модулей вы знаете? (AMD, UMD, CommonJs)
    <https://doka.guide/js/modules>
20. Use strict. <https://doka.guide/js/use-strict/>
21. Что такое рекурсия и для чего используется? <https://doka.guide/js/recursion/>
22. Что такое замыкание и для чего оно используется? <br>
    <https://learn.javascript.ru/closure> <br>
    <https://doka.guide/js/closures/>
23. Отличия в привязке контекста, с помощью bind, call, apply <https://learn.javascript.ru/event-bubbling>
24. Что такое лексическое окружение? <https://learn.javascript.ru/closure#leksicheskoe-okruzhenie>
25. Чем отличается push от unshift? Какой из них быстрее?
26. Отличия function expression и function declaration
    <https://learn.javascript.ru/function-declaration-expression>
27. Что такое Event Loop? <http://learn.javascript.ru/event-loop>
28. Что такое WebAssembly? <https://habr.com/ru/company/ruvds/blog/343568/>
29. Что такое hoisting в JavaScript?
    <https://learn.javascript.ru/event-bubbling>
30. В чем суть стрелочной функции? Ее отличия от обычной функции? <https://doka.guide/js/function/#obychnye-i-strelochnye-funkcii>
31. Иммутабельность, что это и для чего нужна? <https://doka.guide/js/ref-type-vs-value-type/#mutacii-i-neizmenyaemost>
32. WebRTC? <https://habr.com/ru/company/timeweb/blog/656947/>
33. Как можно хранить данные в браузере? <https://doka.guide/js/browsers-storages/>
34. Веб-воркеры и сервис-вореры? <br>
    <https://medium.com/@vKuka/%D0%B2%D0%B5%D0%B1-%D0%B2%D0%BE%D1%80%D0%BA%D0%B5%D1%80%D1%8B-%D1%81%D0%B5%D1%80%D0%B2%D0%B8%D1%81-%D0%B2%D0%BE%D1%80%D0%BA%D0%B5%D1%80%D1%8B-%D0%B8-%D0%B2%D0%BE%D1%80%D0%BA%D0%BB%D0%B5%D1%82%D1%8B-1e2f561312fd> <br>
    <https://habr.com/ru/company/ruvds/blog/349858/>
35. Что такое чистая функция? Является ли функция делающая запрос чистой? <https://qna.habr.com/q/509188>
36. Мемоизация, что это? <https://habr.com/ru/company/ruvds/blog/332384/>
37. Понятие каррирования функции <br>
    <https://learn.javascript.ru/currying-partials> <br>
    <https://habr.com/ru/company/ruvds/blog/427295/>
38. Глубокое и неглубокое копирование объектов? <br>
    <https://medium.com/@stasonmars/%D0%BA%D0%BE%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%B2-%D0%B2-javascript-d25c261a7aff> <br>
    <https://habr.com/ru/post/480786/>
39. Какие коллекции в JavaScript вы знаете? С какими работали? Приведите пример
    использования и основные плюсы какой-либо коллекции? <br>
    <https://doka.guide/js/set/> <br>
    <https://doka.guide/js/map/> <br>
    <https://learn.javascript.ru/weakmap-weakset>
40. В чем отличие Long-polling HTTP от WebSocket. Что лучше использовать? <br>
    <https://qna.habr.com/q/265184> <br>
    <https://ru.stackoverflow.com/questions/536784/%D0%A7%D1%82%D0%BE-%D1%82%D0%B0%D0%BA%D0%BE%D0%B5-html5-websocket-long-short-polling-ajax-webrtc-server-sent-events> <br>
    <https://ably.com/blog/websockets-vs-long-polling>
