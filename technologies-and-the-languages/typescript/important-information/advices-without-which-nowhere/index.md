# Советы без которых никак

## Предпочитайте прямую типизацию параметров и результата работы функции перегрузке

Вы можете сделать много объявлений функции, но при этом указать лишь один вариант выполнения:

```ts
function add(a: number, b: number): number;
function add(a: string, b: string): string;

function add(a, b) {
  return a + b;
}
const three = add(1, 2); // Тип является числом
const twelve = add("1", "2"); // Тип является строкой
```

Два первых объявления add только сообщают информацию типа.
Когда TypeScript произведет вывод JavaScript-кода, они будут удалены и останется только выполнение.
Здесь есть очень большой подводный камень. Дело в том, что перегрузка в TypeScript способна лишь проверить
удовлетворяют ли параметры функции указанным типам. **Однако, TypeScript перестаёт проверять
возвращаемое значение, которое является результатом работы функции, на соответствия типов**.
Пользуйтесь перегрузкой крайне осторожно.

2. Воспринимайте типы как наборы значений

Рассмотрим следующий пример:

```ts
interface Person {
  name: string;
}

interface Lifespan {
  birth: Date;
  death?: Date;
}

type PersonSpan = Person & Lifespan;
```

Оператор & просчитывает пересечение двух типов. Какого рода значения в этом случае принадлежат типу PersonSpan?
На первый взгляд интерфейсы Person и LifeSpan не имеют общих свойств, поэтому вы можете ожидать получения пустого набора.
Но операции типов применяются к наборам значений, а не к свойствам интерфейса.
Запомните, что значения с дополнительными свойствами относятся к типу.
Поэтому значение, имеющее свойства Person и LifeSpan одновременно, будет принадлежать типу пересечения:

```ts
const ps: PersonSpan = {
  name: "Alan Turing",
  birth: new Date("1912/06/23"),
  death: new Date("1954/06/07"),
};
```

Пересечение свойств является корректным для объединения двух интерфейсов, а не для их пересечения.
Не существует ключей, гарантированно принадлежащих значению типа объединения,
поэтому keyof для этого объединения будет пустым набором (never).

```ts
type K = keyof (Person | Lifespan); // тип never
```

Если вы понимаете следующие выражения, значит вы далеко продвинулись в понимании системы типов Typescript:

```
keyof (A & B) = (keyof A) | (keyof B)
keyof (A | B) = (keyof A) & (keyof B)
```

## Проверяйте пределы исключительных свойств типа

Когда вы назначаете объектный литерал к переменной с объявленным
типом, TypeScript дополнительно убеждается, что она обладает исключительно свойствами этого типа и никакими другими.
По сути, вы провоцируете запуск проверки лишних свойств, благодаря чему можете найти важные
ошибки. Распознавание проверки лишних свойств как отдельного процесса поможет выстроить более ясную сознательную модель
системы типов TypeScript.

```ts
interface Room {
  numDoors: number;
  ceilingHeightFt: number;
}
const r: Room = {
  numDoors: 1,
  ceilingHeightFt: 10,
  elephant: "present",
  // ~~~~~~~~~~~~~~~~~~ Объектный литерал может определять только известные
  // свойства, а 'elephant' не существует в типе 'Room'.
};
```

## Используйте сигнатуры индексов для динамических данных

Объекты в JavaScript отображают строковые ключи в значения любого
типа. TypeScript позволяет производить подобное гибкое отображение при
помощи назначения для типа сигнатуры индекса:

```ts
type Rocket = {
  [property: string]: string;
};

const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,940 kN",
}; // ok
```

Тип ключа должен быть некоторой комбинацией string, number или symbol,
но в основном используется string. Используйте сигнатуры индексов, когда свойства объекта не могут
быть известны до момента выполнения программы. Например, если
вы загружаете их из JSON-файл с переводами для мультиязычности.

## Используйте readonly против ошибок, связанных с изменяемостью

```ts
function arraySum(arr: readonly number[]) {
  let sum = 0;

  for (const num of arr) {
    sum += num;
  }

  return sum;
}
```

```ts
let obj: { readonly [k: string]: number } = {};
```

```ts
interface Outer {
  inner: { x: number };
}

type T = Readonly<Outer>;

// Тип T = {
// readonly inner: {
// x: number;
// };
//
```

Важно обратить внимание на то, что модификатор readonly в третьем примере распространяется на inner, но не на x.
Не существует встроенной поддержки глубоких типов readonly, но для этой цели можно создать
обобщение. Сделать это правильно весьма непросто, поэтому обычно
используют библиотеку. Обобщение DeepReadonly в ts-essentials является одной из возможных реализаций.

## Старайтесь сужать тип и контролировать его

Сужение является процессом, обратным расширению. С его помощью
TypeScript переходит от более широкого типа к более узкому.

1. Простой пример этого процесса — проверка null:

```ts
const el = document.getElementById("foo"); // тип HTMLElement | null

if (el) {
  el; // тип HTMLElement
  el.innerHTML = "Party Time".blink();
} else {
  el; // тип null
  alert("No element #foo");
}
```

2. Использование `inctanceof` или проверка свойств:

```ts
function contains(text: string, search: string | RegExp) {
  if (search instanceof RegExp) {
    search; // тип RegExp
    return !!search.exec(text);
  }

  search; // тип string
  return text.includes(search);
}
```

```ts
interface A {
  a: number;
}
interface B {
  b: number;
}

function pickAB(ab: A | B) {
  if ("a" in ab) {
    ab; // тип A
  } else {
    ab; // тип B
  }

  ab; // тип A | B
}
```

3. Добавление к ним явного тега:

```ts
interface UploadEvent {
  type: "upload";
  filename: string;
  contents: string;
}
interface DownloadEvent {
  type: "download";
  filename: string;
}
type AppEvent = UploadEvent | DownloadEvent;

function handleEvent(e: AppEvent) {
  switch (e.type) {
    case "download":
      console.log(e); // тип DownloadEvent
      break;
    case "upload":
      console.log(e); // тип UploadEvent
      break;
    default:
      const restTypes: never = e;
  }
}
```

Такой распространенный шаблон известен как тип-сумма, или размеченное
объединение.

4. Если TypeScript не может выявить тип, попробуйте ввести пользовательскую функцию:

```ts
function isInputElement(el: HTMLElement): el is HTMLInputElement {
  return "value" in el;
}

function getElementContent(el: HTMLElement) {
  if (isInputElement(el)) {
    el; // тип HTMLInputElement
    return el.value;
  }

  el; // тип HTMLElement
  return el.textContent;
}
```

Внимание: будьте осторожны с использованием таких функций. Внутри них TypeScript отключает очень большую часть статической проверки
типов. Например, данный код тоже будет валиден:

```ts
function isInputElement(el: HTMLElement): el is HTMLInputElement {
  return 1 === 2;
}
```

Очень часто, чтобы поддерживать контракт изменения уточняемых типов, их пишут рядом в одном месте. Или используют библиотеки
runtime валидации по типу [runtypes](https://www.npmjs.com/package/runtypes).

Пример, где это необходимо. Если вы используете filter для отсеивания значений undefined, то
TypeScript этого не примет:

```ts
const members = ["Janet", "Michael"]
  .map((who) => jackson5.find((n) => n === who))
  .filter((who) => who !== undefined); // тип (string | undefined)[]
```

Однако он одобрит использование защиты типа:

```ts
function isDefined<T>(x: T | undefined): x is T {
  return x !== undefined;
}

const members = ["Janet", "Michael"]
  .map((who) => jackson5.find((n) => n === who))
  .filter(isDefined); // тип string[]
```
