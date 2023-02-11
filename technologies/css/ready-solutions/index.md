# Готовые решения

## Ссылки

1. Стилизация кросс браузерных вводимых диапазонов с помощью CSS:
   <https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/>
2. Стилизация input range:
   <https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/>

## Стили для главной оболочки

```css
.wrapper {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  flex: 1 1 100%;
  width: 100%;
  min-height: 100vh;
}
```

## Прятать дефолтный чекбокс

```css
.visually-hidden {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  width: 1px;
  height: 1px;
  margin: -1px;
  border: 0;
  padding: 0;
  clip: rect(0 0 0 0);
  overflow: hidden;
}
```

## Поставить три точки в конце текста

Для одно строкового текста

```css
div {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

Для много строкового текста

```css
div {
  display: -webkit-box;
  overflow: hidden;
  -webkit-line-clamp: 2; // количество строк
  -webkit-box-orient: vertical;
}
```

## Скролл состояние для бургер меню

```css
body {
  overflow: hidden;
}

.burger-container {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1;
  display: block;
  width: 100%;
  height: 100%;
}

.burger-content {
  padding: 30px 15px;
  display: block;
  width: 100%;
  height: 100%;
  overflow: auto;
}
```
