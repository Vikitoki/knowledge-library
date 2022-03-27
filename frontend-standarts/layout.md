## Layout

TODO: добавить файл обнуления стилей
TODO: добавить распространённые стили для проектов

### Box-sizing

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

### Modal and PageScroll

```scss
.html {
    position: relative;
    overflow-x: hidden;
    overflow-y: scroll;
    height: 100%;
    font-size: 100%;
    -webkit-overflow-scrolling: touch;

    &.is-blocked {
        overflow: hidden;
     
        .compensate-scroll {
            padding-right: var(--scrollbar);
        }
    }

    &.is-blocked-touched {
       position: fixed;
       overflow-y: scroll;
       width: 100%;
       height: auto;
    }
}

```

### Container



