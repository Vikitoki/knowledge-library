# Готовые решения

## Функция для предания сообщению цвета

```js
const colorer = (message, color) => `\x1b[3${color}m${message}\x1b[0m`;
```

## Корректное округление

```js
function exactRound(number) {
  Math.round(number * 10) / 10;
}
```

## Полезные функции рандомизации

```js
function randomFloat(min, max) {
  return min + Math.random() * (max - min);
}

function randomInteger(min, max) {s
  let rand = min - 0.5 + Math.random() * (max - min + 1);
  return Math.round(rand);
}
```
