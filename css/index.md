# Оглавление CSS

- [Готовые решения](./ready-solutions.md)

Используй [БЭМ](https://ru.bem.info/methodology/key-concepts/) (в случае если на проекте нету CSS modules или css-in-js)

.block {

`    `$this: &;

`    `position: relative;

`    `width: 100vw;

`    `&\_\_element-bg {

`        `position: absolute;

`        `top: 0;

`        `left: 0

`        `width: 100px;

`        `height: 100px;

`        `background-color: $color-red;



`        `&--right {

`            `left: auto;

`            `right: 0;

`        `}



`        `#{$this}--big & {

`            `width: 200px;

`            `height: 200px;

`        `}

`    `}



`    `&\_\_element-border {

`      `border: 1px solid $color-black;



`      `#{$this}--big & {

`          `border-width: 4px;

`      `}

`    `}

}


### **Не перегружай элементы БЭМ'а.**
Если елемент начинает содержать два или больше смысловых вложений, то это значит, что тебе скорее всего надо выносить такой элемент в отдельный блок.

**Не забывайте, что каждый блок должен жить в своем отдельном файле.**

**плохо:**

navigation.scss

.navigation {

`    `&\_\_list {

`        `...

`    `}



`    `&\_\_item {

`        `....

`    `}



`    `&\_\_link {

`        `...

`    `}



`    `&\_\_item-list {

`        `...

`    `}



`    `&\_\_item-list-link {

`        `...

`    `}



`    `&\_\_item-list-icon {

`        `...

`    `}

}

**хорошо:**

navigation.scss

.navigation {

`    `&\_\_list {

`        `...

`    `}



`    `&\_\_item {

`        `....

`    `}



`    `&\_\_link {

`        `...

`    `}

}

navigation-list.scss

.navigation-list {

`    `...



`    `&\_\_link {

`        `...

`    `}



`    `&\_\_icon {

`        `...

`    `}

}
## **Селекторы в CSS**
- Не используй тяжелые селекторы без реальной обходимости.
- Не используй чистые теги как часть селектора, без реальной обходимости
- Если для разрешения какой то проблемы надо использовать селектор с весом больше чем 20 попугаев, то это хороший повод задуматься над необходимостью что то упростить.



**плохо**

.block {

`    `$this: &;



`    `&\_\_element {

`        `color: $color-white;



`        `.section .section--dark & {

`            `color: $color-black;

`        `}

`    `}

}



**хорошо**

.block {

`    `$this: &;



`    `&\_\_element {

`        `color: $color-white;

`    `}



`    `#{$this}--dark & {

`        `color: $color-black;

`    `}

}


## **Правило порядка стилей в CSS**
Для консистентности мы договорились писать стили в едином порядке, он описан в файле настроек
.stlelintrc
