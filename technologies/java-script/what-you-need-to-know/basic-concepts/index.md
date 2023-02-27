# Основные концепции

## Прототипное наследование

### Пример работы

```js
let animal = {
  eats: true,
  walk() {
    alert("Animal walk");
  },
};

let rabbit = {
  jumps: true,
  __proto__: animal,
};

// walk взят из прототипа
rabbit.walk();
```

### Значение «this».

Неважно, где находится метод: в объекте или его прототипе. При вызове метода, `this` — это всегда объект перед точкой. В результате методы являются общими, а состояние объекта — нет!

```js
// методы animal
let animal = {
  walk() {
    if (!this.isSleeping) {
      alert(`I walk`);
    }
  },
  sleep() {
    this.isSleeping = true;
  },
};

let rabbit = {
  name: "White Rabbit",
  __proto__: animal,
};

// модифицирует rabbit.isSleeping
rabbit.sleep();

alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined (нет такого свойства в прототипе)
```

### F.prototype

Если в F.prototype содержится объект, оператор `new` устанавливает его в качестве `[[Prototype]]` для нового объекта. Установка `Rabbit.prototype = animal` буквально говорит интерпретатору следующее: "При создании объекта через `new Rabbit()` запиши ему animal в `[[Prototype]]`".

```js
let animal = {
  eats: true,
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal

alert(rabbit.eats); // true
```

У каждой функции по умолчанию уже есть свойство "prototype". По умолчанию, оно представляет из себя объект с единственным свойством constructor, которое ссылается на функцию-конструктор.

```js
function Rabbit() {}
// по умолчанию:
// Rabbit.prototype = { constructor: Rabbit }

alert(Rabbit.prototype.constructor == Rabbit); // true
```

В обычных объектах prototype не является чем-то особенным!

```js
let user = {
  name: "John",
  prototype: "Bla-bla", // никакой магии нет - обычное свойство
};
```

### Дополнения

1. Свойство `__proto__` немного устарело, оно существует по историческим причинам. Современный JavaScript предполагает, что мы должны использовать функции `Object.getPrototypeOf` и `Object.setPrototypeOf` вместо того, чтобы получать/устанавливать прототип. Мы также рассмотрим эти функции позже.

2. `__proto__` — не то же самое, что внутреннее свойство `[[Prototype]]`. Это геттер/сеттер для `[[Prototype]]`.

3. Прототип используется только если свойство не найдено в первичном объекте

   ```js
   let animal = {
     eats: true,
     walk() {
       /* этот метод не будет использоваться в rabbit */
     },
   };

   let rabbit = {
     __proto__: animal,
   };

   rabbit.walk = function () {
     alert("Rabbit! Bounce-bounce!");
   };

   rabbit.walk(); // Rabbit! Bounce-bounce!
   ```

4. Свойства и методы `Object.prototype` **не перечислимы** из-за того, что у прототипа внутренний флаг `enumerable` стоит `false`.

## Асинхронное программирование

### На колбэках

Эта функция загружает на страницу новый скрипт. Когда в тело документа добавится конструкция `<script src="…">`, браузер загрузит скрипт и выполнит его.

```js
function loadScript(src, callback) {
  let script = document.createElement("script");
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () =>
    callback(new Error(`Не удалось загрузить скрипт ${src}`));

  document.head.append(script);
}

loadScript(
  "https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js",
  (script) => {
    alert(`Здорово, скрипт ${script.src} загрузился`);
    alert(_); // функция, объявленная в загруженном скрипте
  }
);
```

### На Promise

#### Синтаксис создания:

```js
let promise = new Promise(function (resolve, reject) {
  // функция-исполнитель (executor)
  // "певец"
});
```

#### Варианты использования

1. Параллельный

   ```js
   let promise = loadScript(
     "https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"
   );

   promise.then(
     (script) => alert(`${script.src} загружен!`),
     (error) => alert(`Ошибка: ${error.message}`)
   );

   promise.then((script) => alert("Ещё один обработчик..."));
   ```

2. Последовательный

   ```js
   let promise = loadScript(
     "https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js"
   );

   promise
     .then(
       (script) => alert(`${script.src} загружен!`),
       (error) => alert(`Ошибка: ${error.message}`)
     )
     .then((script) => alert("Ещё один обработчик..."));
   ```

В первом случае обработчики `.then` не передают друг другу результаты своего выполнения, а действуют независимо. Гораздо более распространён второй вариант использования.

#### Thenable

Если быть более точными, обработчик может возвращать не именно промис, а любой объект, содержащий метод `.then`, такие объекты называют «thenable», и этот объект будет обработан как промис.

Смысл в том, что сторонние библиотеки могут создавать свои собственные совместимые с промисами объекты. Они могут иметь свои наборы методов и при этом быть совместимыми со встроенными промисами, так как реализуют метод `.then`.

Например:

```js
class Thenable {
  constructor(num) {
    this.num = num;
  }
  then(resolve, reject) {
    alert(resolve); // function() { native code }
    // будет успешно выполнено с аргументом this.num*2 через 1 секунду
    setTimeout(() => resolve(this.num * 2), 1000); // (**)
  }
}

new Promise((resolve) => resolve(1))
  .then((result) => {
    return new Thenable(result); // (*)
  })
  .then(alert); // показывает 2 через 1000мс
```

#### Полезно знать

1. Свойства Promise.state и Promise.result – внутренние! Мы не имеем к ним прямого доступа. Для обработки результата следует использовать методы `.then`,`.catch` и `.finally`.

2. Особенности `.finally`:

   - Обработчик не получает результат предыдущего обработчика (у него нет аргументов).
   - Вместо этого этот результат передается следующему подходящему обработчику. Если обработчик `.finally` возвращает что-то, это игнорируется.
   - Когда `.finally` выдает ошибку, выполнение переходит к ближайшему обработчику ошибок.

3. Исполнитель должен вызвать что-то одно: `resolve` или `reject`. Состояние промиса может быть изменено только один раз. Все последующие вызовы будут проигнорированы:

   ```js
   let promise = new Promise(function (resolve, reject) {
     resolve("done");

     reject(new Error("…")); // игнорируется
     setTimeout(() => resolve("…")); // игнорируется
   });
   ```

4. Обработка промисов всегда асинхронная, т.к. все действия промисов проходят через внутреннюю очередь «promise jobs», так называемую «очередь микрозадач (microtask queue)» (термин v8). Таким образом, обработчики `.then` `.catch` и `.finally` вызываются после выполнения текущего кода.Если нам нужно гарантировать выполнение какого-то кода после `.then` `.catch` и `.finally`, то лучше всего добавить его вызов в цепочку `.then`.

### На async/await

Ключевое слово `async` перед объявлением функции:

1. Обязывает её всегда возвращать промис.
2. Позволяет использовать `await` в теле этой функции.

Ключевое слово `await` перед промисом заставит JavaScript дождаться его выполнения, после чего:

1. Если промис завершается с ошибкой, будет сгенерировано исключение, как если бы на этом месте находилось `throw`.
2. Иначе вернётся результат промиса.

Вместе они предоставляют отличный каркас для написания асинхронного кода. Такой код легко и писать, и читать.
