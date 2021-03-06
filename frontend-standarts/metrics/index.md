# Метрики

[Хорошая статья по теме](https://habr.com/ru/company/ruvds/blog/462005/)

## Google page speed

Клиентов, зачастую, интересует показатели с данного сервиса. 
Это хороший инструмент, который позволяет понять, в каком месте у сайта есть проблемы. Почти.
Хорошие показатели привлекают новых клиентов.


## Google Lighthouse

Lighthouse — это опенсорсный проект, которым занимается специальная команда,
выделенная из числа разработчиков Google Chrome. За последние пару лет Lighthouse стал стандартным 
бесплатным инструментом для анализа производительности сайтов.

Lighthouse использует средства Chrome по удалённой отладке (Chrome Remote Debugging Protocol) 
для чтения сведений о сетевых запросах, для измерения производительности JavaScript-кода, для проверки соблюдения 
стандартов доступности содержимого страницы. Этот инструмент измеряет временные показатели, ориентированные 
на особенности восприятия страницы пользователями. Среди них, например, First Contentful Paint (Время загрузки первого контента),
Time to Interactive (Время загрузки для взаимодействия) и Speed Index (Индекс скорости загрузки).

## Основные проблемы, связанные с Google page speed

1. Неиспользуемый код. Например, если код и стили для компонента присутствуют в файле, но самого компонента на странице нет. 
При накоплении таких моментов в коде происходит колоссальное снижение показателей.

2. Прыгающие стили или стили загружающиеся рывками. Часто при плохой оптимизации, разделении файлов при загрузке страницы вы можете успеть увидеть нестилизованный сайт,
увидеть HTML, в котором отсутствуют стили. Через определённое время спустя они появляются на странице
и происходят очень сильные рывки, которые влияют на то, что видит пользователь. К счастью в современном фронтенде существуют подходы
и целые технологии направленные на решение данной проблемы. Примеры таких подходов:

    - **“Critical”.** Подход, при котором, в css файлы, являющиеся критическими для страницы, подключаются в [первую очередь](https://doka.guide/css/adding-styles/#vstroennye-stili).

    - “Preload chunks“. <link rel=”preload” href=”…” as=”style” /> Таковой подход значительно снизит нагрузку на бэкенд,
    в том плане, что им не прийдется обрабатывать двойное
    существование одних и тех же страниц,
    с разницей в наличии инлайновых стилей, с обрабуткой кук, что уже есть, а что нет. По сути NextJS так и делает, там нету прям космической магии.

TODO: поискать такие решения и скинуть ссылки на них
