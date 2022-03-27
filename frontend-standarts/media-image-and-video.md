## Media Image and Video

### Aspect Ratio Boxes

На момент написания раздела функция CSS aspect-ratio поддерживается только в тестовой версии firefox. 
Чтобы заставить контейнер сохранять нужные пропорции, мы используем процентный padding.
Padding в процентах всегда зависит от ширины родителя, при этом не имеет значения в какую сторону он направлен. 
Простыми словами, padding-top: 10% будет равен 10-ой части ширины родителя. 
Всё что остаётся сделать - вписать в контейнер дочерние элементы, с помощью position: absolute.

```scss
// Разделите высоту на ширину и заверните в percentage.

.parent {
  position: relative;
  --percenatege: 9/16; // 56.25%
  
  &::before {
    content: '';
    display: block;
    padding-top: var(--percenatege);
  }


  .child-image, .child-video {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
}
```

### Object fit 

По умолчанию блоки img, video, picture и др. не имеют функциональной возможности 
автоматически подстраиваться под контент, расположенный в них. 
Это значит, что при изменении ширины и высоты вашего устройства вы можете увидеть, как ваша вёрстка будет ломаться.
Чтобы такого не происходило, мы используем данные css правила:

TODO: добавить про css блок object-fit.

### Lazy load

TODO: добавить информации и решений про Lazy load

CSS

```scss
.lazyLoading {
  opacity: 0;
}

.lazyLoaded {
    opacity: 1;
    transition: opacity 300ms;
}
```

### Background video

TODO: добавить решение для Background video
