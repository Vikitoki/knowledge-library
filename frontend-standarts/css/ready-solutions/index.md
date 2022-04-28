# Готовые решения

## Ссылки

1. Стилизация кросс браузерных вводимых диапазонов с помощью CSS: 
<https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/>
2. Стилизация input range: 
<https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/>

---

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

---

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

---
