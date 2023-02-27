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

function randomInteger(min, max) {
  s;
  let rand = min - 0.5 + Math.random() * (max - min + 1);
  return Math.round(rand);
}
```

## Декораторы

1. Переадресация вызова выполняется с помощью apply:

   ```js
   let wrapper = function (original, arguments) {
     return original.apply(this, arguments);
   };
   ```

2. Декоратор "Шпион"

   Использование:

   ```js
   function work(a, b) {
     alert(a + b); // произвольная функция или метод
   }

   work = spy(work);

   work(1, 2); // 3
   work(4, 5); // 9

   for (let args of work.calls) {
     alert("call:" + args.join()); // "call:1,2", "call:4,5"
   }
   ```

   Реализация:

   ```js
   function spy(func) {
     function wrapper(...args) {
       wrapper.calls.push(args);
       return func.apply(this, args);
     }

     wrapper.calls = [];

     return wrapper;
   }
   ```

3. Декоратор "Задерживающий"

   Использование:

   ```js
   function f(x) {
     alert(x);
   }

   // создаём обёртки
   let f1000 = delay(f, 1000);
   let f1500 = delay(f, 1500);

   f1000("test"); // показывает "test" после 1000 мс
   f1500("test"); // показывает "test" после 1500 мс
   ```

   Реализация:

   ```js
   function delay(f, ms) {
     return function () {
       setTimeout(() => f.apply(this, arguments), ms);
     };
   }

   // Или

   function delay(f, ms) {
     return function (...args) {
       let savedThis = this; // сохраняем this в промежуточную переменную

       setTimeout(function () {
         f.apply(savedThis, args); // используем её
       }, ms);
     };
   }
   ```

4. Декоратор "debounce"

   Использование:

   ```js
   let a = debounce(alert, 100);

   a(1); // выполняется немедленно
   a(2); // проигнорирован

   let b = debounce(alert, 1000);

   setTimeout(() => b(3), 100); // выполняется
   setTimeout(() => b(4), 1100); // проигнорирован (прошло только 900 мс от последнего вызова)
   setTimeout(() => b(5), 1500); // выполняется
   ```

   Реализация:

   ```js
   function debounce(f, ms) {
     let isCooldown = false;

     return function (...args) {
       if (isCooldown) return;

       f.apply(this, args);

       isCooldown = true;

       setTimeout(() => (isCooldown = false), ms);
     };
   }
   ```

5. Декоратор "throttle"

   Использование:

   ```js
   function f(a) {
     console.log(a);
   }

   // f1000 передаёт вызовы f максимум раз в 1000 мс
   let f1000 = throttle(f, 1000);

   f1000(1); // показывает 1
   f1000(2); // (ограничение, 1000 мс ещё нет)
   f1000(3); // (ограничение, 1000 мс ещё нет)

   // когда 1000 мс истекли ...
   // ...выводим 3, промежуточное значение 2 было проигнорировано
   ```

   Реализация:

   ```js
   function throttle(func, ms) {
     let isThrottled = false;
     let savedArgs = null;
     let savedThis = null;

     function wrapper(...args) {
       if (isThrottled) {
         // (2)
         savedArgs = args;
         savedThis = this;

         return;
       }

       func.apply(this, args); // (1)

       isThrottled = true;

       setTimeout(function () {
         isThrottled = false; // (3)

         if (savedArgs) {
           wrapper.apply(savedThis, savedArgs);

           savedArgs = null;
           savedThis = null;
         }
       }, ms);
     }

     return wrapper;
   }
   ```

## Частичное применение

Применяется, когда мы не хотим повторять один и тот же аргумент много раз. Например, если у нас есть функция `send(from, to)` и `from` всё время будет одинаков для нашей задачи, то мы можем создать частично применённую функцию и дальше работать с ней.

1. С привязкой контекста

   Реализация:

   ```js
   function mul(a, b) {
     return a * b;
   }
   ```

   Использование:

   ```js
   let double = mul.bind(null, 2);

   alert(double(3)); // = mul(2, 3) = 6
   alert(double(4)); // = mul(2, 4) = 8
   alert(double(5)); // = mul(2, 5) = 10
   ```

2. Без привязки контекста

   Реализация:

   ```js
   function partial(func, ...argsBound) {
     return function (...args) {
       return func.call(this, ...argsBound, ...args);
     };
   }
   ```

   Использование:

   ```js
   let user = {
     firstName: "John",
     say(time, phrase) {
       alert(`[${time}] ${this.firstName}: ${phrase}!`);
     },
   };

   // добавляем частично применённый метод с фиксированным временем
   user.sayNow = partial(
     user.say,
     new Date().getHours() + ":" + new Date().getMinutes()
   );

   user.sayNow("Hello"); // например, [10:00] John: Hello!
   ```

## Обработка исключений на уровне документа

```js
window.addEventListener("unhandledrejection", function (event) {
  // объект события имеет два специальных свойства:
  alert(event.promise); // [object Promise] - промис, который сгенерировал ошибку
  alert(event.reason); // Error: Ошибка! - объект ошибки, которая не была обработана
});
```
