# CSS

1. Стилизация кросс браузерных вводимых диапазонов с помощью CSS: 
<https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/>

---

## Класс, чтобы прятать дефолтный чекбокс

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
