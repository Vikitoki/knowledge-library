# HTML

---

## Разметка меню приложения

```html
<nav class="navigation">
  <ul class="menu" role="menu">
    <li role="menuitem">
      <a href="#">Ссылка</a>
    </li>
  </ul>
</nav>
```
---

## Определение footer через voice over

```html
<footer class="navigation" role="contentinfo"></footer>
```
---

## Реализация картинки для разных медиа выражений

```html
<picture>
  <source
    srcset="picture-wide@1x.png 1x, picture-wide@1x.png 2x"
    media="(min-width: 600px)"
    type="image/webp"
  />
  <source
    srcset="picture-wide@1x.png 1x, picture-wide@1x.png 2x"
    media="(min-width: 600px)"
  />
  <source srcset="picture@1x.webp 1x, picture@1x.webp 2x" type="image/webp" />
  <img
    src="picture@1x.png"
    srcset="picture@2x.png 2x"
    alt="Девушка пьёт кофе"
  />
</picture>
```
