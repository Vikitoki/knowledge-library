# Вопросы для собеседования

## Теоретические технические вопросы

### CSS & HTML

1. Что такое препроцессор? Приведите примеры препроцессоров. (SCSS, SASS, Less)
2. Что такое постпроцессор?
3. Какие знаете методологии верстки? (БЭМ, SMACSS)
4. Основные правила БЭМ. Максимальная вложенность по БЭМ. Где в БЭМ может быть
каскад? Где можно использовать/где нельзя: <br>
<https://yandex.com/dev/bem/> <br>
<https://habr.com/ru/company/yandex/blog/276035/>
5. Box-sizing. Чем отличается значение border-box от остальных: <https://doka.guide/css/box-sizing/>
6. Что такое уникальность (специфичность) селектора: <https://doka.guide/css/specificity/>
7. В чем отличие между transform: translate и position свойствами? <https://webformyself.com/css-ot-a-do-ya-raznica-mezhdu-translate-i-position-relative/>
8. Как сделать анимацию в браузере через css? <https://doka.guide/css/animation/>
9. Расскажите как работает z-index? Если z-index не задан? <https://doka.guide/css/z-index/>
10. Зачем нужны media выражения? <https://doka.guide/css/media/>
11. Ретина. Что это такое, как используются варианты изображения для обычного экрана и
для ретины в html|css: <br>
<https://habr.com/ru/post/150071/> <br>
<https://htmlacademy.ru/blog/boost/frontend/retina>
12. SVG. Для чего нужен, как используется, приемы применения: <br>
<https://doka.guide/html/svg/> <br>
<https://ru.hexlet.io/blog/posts/kak-rabotat-s-formatom-svg-rukovodstvo-dlya-nachinayuschih-veb-razrabotchikov> 
13. Чем отличается {display: none} от {visibility: hidden , opacity: 0}
14. Cвойство Position. Назовите возможные свойства position: <https://doka.guide/css/position/>
15. DOM-интерфейс и работа с ним: <https://doka.guide/js/dom/>
16. Как получить тэг html? <https://doka.guide/js/element/>
17. Cобытия жизненного цикла страницы: <https://doka.guide/js/event-load-and-domcontentloaded/>
18. Как можно перебрать коллекцию html элементов (*) 
19. Как перевести html коллекцию в массив (*) : <https://doka.guide/js/array-from/>
20. Атрибуты тега <script>: <https://doka.guide/html/script/>
21. Чем отличаются reflow и repaint? <br>
<https://habr.com/ru/post/224187/> <br>
<https://doka.guide/js/how-the-browser-creates-pages/>

### Java Script

1. В чём заключаются различия var, const и let? <https://doka.guide/js/var-let/>
2. Какие типы данных есть в JavaScript? <https://learn.javascript.ru/types>
3. Какие из типов данных являются ссылочными типами в JavaScript? <https://learn.javascript.ru/object-reference>
4. Тип данных symbol: что такое, когда и для чего используется? <https://doka.guide/js/symbol/>
5. Генераторы/итераторы. <https://learn.javascript.ru/generators>
6. Как работает наследование в JS? <https://learn.javascript.ru/class-inheritance>
7. Прототипная модель. Что это? Как организуется родительская цепочка?
<https://learn.javascript.ru/class-inheritance>
8. Чем отличается __proto__ от prototype? <https://learn.javascript.ru/new-prototype>
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


### TypeScript

1. Что такое TypeScript? Зачем его использовать вместо JavaScript?
2. Типы данных TypeScript?
3. Что за операторы & и |, их особенности и различия в TypeScript?
4. Расскажите об обобщенных типах в TypeScript? (generics)
5. Поддерживает ли TypeScript принципы ООП? Какие из них вы знаете?
6. Как в TypeScript реализовать свойство класса являющуюся константой?
7. Расскажи про классы? Если я хочу переопределить в дочернем классе конструктор, есть
ли с этим какая-то особенность? let, const? Если я в const запишу объект, я смогу его
менять?
8. Что представляют собой .map-файлы в TypeScript?
9. Что такое геттеры и сеттеры в TypeScript?
10. Можно ли использовать TypeScript на бэке?
11. Расскажите об основных компонентах TypeScript? (язык, компилятор, вспомогательные
инструменты)
12. Декораторы в TypeScript?
13. Можно ли в TypeScript использовать строго типизированные функции использовать в
качестве параметров?
14. Как сделать классы объявленные внутри модуля доступными извне?
15. Поддерживает ли TypeScript перегрузку функций? Как это реализовать?
16. В чем разница между interface и type в TypeScript?
17. Когда в TypeScript используется ключевое слово declare?
18. Как сводить к определенному типу в TypeScript? (type assertion)
19. Как проверить тип объекта в TypeScript?
20. Расскажите подробнее о enum(перечисления)?
21. Что такое union type?
22. Что такое утиная типизация?
23. Когда увидите полезность TypeScript

