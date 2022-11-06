# Общеприменимые

1. Функция для предания сообщению цвета

   ```js
   const colorer = (message, color) => `\x1b[3${color}m${message}\x1b[0m`;
   ```
