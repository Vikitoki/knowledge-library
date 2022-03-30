# Общее

## Web Vitals

Немного терминологии:

**TTFB.** Time To First Byte - время от момента запроса, до первого полученного байта.
Зависит от хостинга, CDN и состояния кеширования. Как правило, не является задачей UI. Улучшить этот показатель очень не просто.
Однако, нужно знать о месте хранения ассетов проекта (картинки, шрифты, иконки и т.д). Если
CDN отдают файлы быстрее чем хостинг, то следует вынести данные файлы именно туда.

**FCP.** First Contentful Paint - время первого кадра, который заполнит весь вьюпорт.
Момент, когда пользователь увидит хоть что-то. Зависит от размера и способа подключения css стилей.

**LCP.** Largest Contentful Paint - время до появления самого крупного элемента на странице, например картинки.
Зависит от размера js файлов, скорости работы парсера движка браузера.

**TBT.** Total Blocking Time - время, в течении которого, пользователь
не может взаимодействовать со страницей. Чаще всего, данный показатель определяется временем загрузки js файлов и загрузкой html
документа, после парсинга.

**CLS.** Cumulative Layout Shift - время, фиксируемое во время последнего изменения контента страницы, при котором
меняется высота вьюпорта.

**FID.** First Input Delay - время, указывающее на то, как скоро пользователь сможет взаимодействовать со страницей (например скролить).
Зависит от размера стилей и js.

## Files structure

Стандартная структура папок для стека Microsoft (C# + js/ts + scss)

```
project-folder /
package.json
webpack.config.js
.eslintrc
.stylelintrc
.prettierrc
...etc project root files...
assets /
    index.js
    init.js
    components/
        my-component/
            index.js
            js/
                my-component.component.js
                my-support-component.component.js
                helpers.js
                enums.js
            scss/
                index.scss
                my-component.scss
                my-support-component.scss

    general/
        images/
            ...some images..
        svg/
            ...some svg icons for importing in svg sprite...
        fonts/
            ...some fonts for project...
        favicons/
            ...some favicons...
        js/
            app.js
            device.js
            helpers/
                object-helper.js
                date-helper.js
        scss/
            index.scss
            buttons.scss
            font-face.scss
            grid.scss
            helpers.scss
            layout.scss
            spaces.scss
            icons.scss
            typography.scss
            settings/
                index.scss
                variables.scss
                breakpoints.scss
                mixins.scss
                fonts.scss

webpack/
    webpack.base.js
    webpack.dev.js
    webpack.prod.js
    webpack.server.js
    webpack.static.js
    webpack.entry.js

dist/
    ...here will be results off assets build...

nodemodules/
    ...some plugins...
```

## Способы автоматического соблюдения

- [Eslintrc](assets/eslintrc)
- [Prettierrc](assets/prettierrc)
- [Stylelintrc](assets/stylelintrc)

## Почему размер бандла так важен?

TODO: подумать, нужен ли этот раздел

На первый взгляд, кажется, что условные 1.5мб js не так уж и много,
да на любой странице картинка может оказаться больше чем бандл, а ведь картинок много.
Но загвоздка в том, что js это не просто статичные данные, как картинка,
это нагрузка на память и процессор устройства.
Да, на компе проблем может не наблюдаться, но вот на мобилке все убдет очень плохо.
Мобилки априори слабее компов, у них ниже скорость подключения к интернетику.
Медленно приехавшая лэзи лоад картинка, не заблокирует пользователя при оформлении заказа,
и если не приедет вовсе, то не сломает сайт. Картинке, после того как приехала, останется всего лишь прогрузиться.
А вот js бандл должен еще отработать, отработать в правильном порядке, записать в память все необходимые переменные и т.д.

И поэтому, чем меньше бандл, тем меньше устройству нужно будет выполнить операций.
