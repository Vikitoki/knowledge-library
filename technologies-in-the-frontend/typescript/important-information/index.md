# Важная информация

## Введение

Typescript базируется на 4 вещах:

1. **Структурная типизация**. Условно у нас есть две сущности, которые одинаково летают, одинаково крякают, но при этом они порождены
от разных сущностей. В мире JavaScript очень много библиотек, которые имеют похожие системы типов. Поэтому, было бы очень плохо
если бы между собой они порождали конфликты. 
Компилятор Typescript считает типы совместимыми, если сопоставляемый тип имеет все признаки типа, с которым сопоставляется.
Пример:

```ts
class USD {
  constructor(public amount: number) {
  }
}

class EUR {
  constructor(public amount: number) {
  }
}

const walletFirst: USD = new USD(100);
const walletSecond: EUR = walletFirst;
```

2. **Aliasing (передача по ссылке)**. Пример:


```ts
type Version = {
  value: number
}

type RelaxedVersion = {
  value: number | string
}

const versionFirst: Version = {
  value: 1
}

const versionSecond: RelaxedVersion = versionFirst
versionSecond.value = 'something'

versionFirst.value.toFixed(2);
```

Данный код позволит уронить программу во время выполнения. Думаю, что здесь должно быть понятно - это дизайн решение было взято примяком
из JavaScript. В данном случае, Typescript никак не мешает нам делать то, что задумано разработчиком. Вместо строгой проверки сущностей 
на предмет более широкого типа, Typescript в строке `const versionSecond: RelaxedVersion = versionFirst` доверяет нам, что мы не обманули его
и в переменной всегда будет лежать множество значений указанного типа.

3. **Мутабельность**. В JavaScript очень много вещей, которые поддаются мутациям. Как самый явный пример - это не примитивы, 
которые передаются по ссылке, а как более интересный пример - DOM дерево. Typescript пошёл по пути поддержки такого подхода. 
Здесь встаёт вопрос: можем ли мы сделать что-то, для повышения надёжности нашего кода с точки зрения типов Typescript. 
Ответ прост - да можем, при помощи применения концепций парадигмы функционального программирования.

4. **Иерархия типов**. Прекрасные материалы про соотношения типов между собой:

- <https://habr.com/ru/post/477448/>
- <https://habr.com/ru/post/531030/>


## Предпочитайте прямую типизацию параметров и результата работы функции перегрузке

Вы можете сделать много объявлений функции, но при этом указать лишь один вариант выполнения:

```ts
function add(a: number, b: number): number;
function add(a: string, b: string): string;

function add(a, b) {
return a + b;
}
const three = add(1, 2); // Тип является числом
const twelve = add('1', '2'); // Тип является строкой
```

Два первых объявления add только сообщают информацию типа. 
Когда TypeScript произведет вывод JavaScript-кода, они будут удалены и останется только выполнение.
Здесь есть очень большой подводный камень. Дело в том, что перегрузка в TypeScript способна лишь проверить 
удовлетворяют ли параметры функции указанным типам. **Однако, TypeScript перестаёт проверять
возвращаемое значение, которое является результатом работы функции, на соответствия типов**. 
Пользуйтесь перегрузкой крайне осторожно.

## Воспринимайте типы как наборы значений

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
name: 'Alan Turing',
birth: new Date('1912/06/23'),
death: new Date('1954/06/07'),
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

## Хамелеонные конструкции Typescript

Существуют конструкции, которые имеют разное значение в зависимости 
от того в каком пространстве они находятся (типов или значений):

1. Оператор typeof

```ts
const p = { first: 'Jane', last: 'Jacobs' };

type T1 = typeof p; // Тип Person
type T2 = typeof email;
 // Тип (p: Person, субъект: string, тело: string) => Response

const v1 = typeof p; // значение object
const v2 = typeof email; // значение function
```

2. Методы доступа к свойствам по ключу:

```ts
const p = { first: 'Jane', last: 'Jacobs' };

const first: Person['first'] = p['first']; // значение Jane

type PersonEl = Person['first' | 'last']; // Тип string
type Tuple = [string, number, Date];
type TupleEl = Tuple[number]; // Тип string | number | Date
```

3. Метод this в пространстве значений является ключевым словом
JavaScript (правило 49). В пространстве типов this выступает как
TypeScript-тип this (а-ля «полиморфный this»). Он полезен при выполнении цепочек методов для подклассов.

```ts
const elNull = document.getElementById('foo'); // Тип HTMLElement | null
const el = document.getElementById('foo')!; // Тип HTMLElement
```

4. В качестве префикса ! выступает как оператор единичного отрицания
в JavaScript (а-ля «не»). Будучи суффиксом, он означает ненулевое утверждение типа.

5. В пространстве значений & и | являются побитовыми операциями «и»
и «или». В пространстве типов они выступают как операторы объединения и пересечения.

6. Ключевое слово const вводит новую переменную, но as const изменяет
выведенный тип на постоянный.

7. extends может либо определять подкласс (класс А расширяет класс B)
или подтип (интерфейс A расширяет интерфейс B), либо ставить ограничение на обобщенный тип (Generic<T extends number>).

8. in может являться либо частью цикла for…in, либо отображаемым типом.


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
 elephant: 'present',
// ~~~~~~~~~~~~~~~~~~ Объектный литерал может определять только известные
// свойства, а 'elephant' не существует в типе 'Room'.
};
```

## Разница между Type и Interface

Граница между этими вариантами с течением времени размылась настолько, 
что в некоторых ситуациях можно использовать и то и другое. 
Однако есть небольшие или существенные различия:

1. Определение типов функции.

```ts
type TFn = (x: number) => string;

 interface IFn {
(x: number): string;
}
```

Псевдоним типа выглядит более естественно для этого простейшего типа
функции.

2. Interface может расширять Type и наоборот:

```ts
interface IStateWithPop extends TState {
 population: number;
}

type TStateWithPop = IState & { population: number; }
```

Оговорка заключается в том, что
Interface не может расширять сложные типы вроде типов объединений.
Если вам нужно именно это, придется использовать Type и &. 


3. Cуществуют типы объединения, но не существует интерфейсов объединения:

```ts
type AorB = 'a' | 'b';
```

В целом Type имеет больше возможностей, чем Interface. 
Он может выступать в качестве объединения, а также пользоваться более продвинутыми возможностями вроде
отображения или условных типов. 

4. Помимо этого, с помощью Type можно ясно выразить кортежи и типы массивов:

```ts
type Pair = [number, number];
type StringList = string[];
type NamedNums = [string, ...number[]];
```

Тем не менее у interface есть некоторые возможности, отсутствующие у type.

5. Interface может быть дополнен:

```ts
interface IState {
 name: string;
 capital: string;
}

interface IState {
 population: number;
}

const wyoming: IState = {
 name: 'Wyoming',
 capital: 'Cheyenne',
 population: 500_000
}; // ok
```

Это называется объединением деклараций. Главным образом оно используется с файлами деклараций типов. 
Для их поддержки уместно использовать interface, поскольку в декларациях типов могут быть пропуски, 
которые должны заполнять пользователи. TypeScript использует слияние, чтобы получать разные типы для разных
версий стандартной библиотеки JavaScript.

Выводы:

Вернемся к вопросу выбора между type и interface. Для сложных типов
выбора нет: необходимо использовать псевдоним типа. Но что насчет
более простых типов объектов, которые могут быть представлены и другим
путем? Чтобы ответить на этот вопрос, следует учитывать согласованность и возможное дополнение. 
Если вы работаете с кодом, который постоянно использует interface, то придерживайтесь interface. Если же
в нем применяется type, используйте type.

В проектах, не имеющих установленного стиля, вам следует поразмыслить
над возможностью дополнения. Если вы опубликуете декларации типов для
API, пользователям будет удобно вставлять новые поля через interface при
изменении API. Однако для типа, который используется внутри проекта,
слияние деклараций может оказаться ошибкой, поэтому отдайте предпочтение type


## Используйте сигнатуры индексов для динамических данных

Объекты в JavaScript отображают строковые ключи в значения любого
типа. TypeScript позволяет производить подобное гибкое отображение при
помощи назначения для типа сигнатуры индекса:

```ts
type Rocket = {
  [property: string]: string
};

const rocket: Rocket = {
 name: 'Falcon 9',
 variant: 'v1.0',
 thrust: '4,940 kN',
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
let obj: {readonly [k: string]: number} = {};
```
```ts
interface Outer {
 inner: { x: number; }
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
const el = document.getElementById('foo'); // тип HTMLElement | null

if (el) {
 el // тип HTMLElement
 el.innerHTML = 'Party Time'.blink();
} else {
 el // тип null
 alert('No element #foo');
}
```

2. Использование `inctanceof` или проверка свойств:

```ts
function contains(text: string, search: string|RegExp) {
  
 if (search instanceof RegExp) {
 search // тип RegExp
 return !!search.exec(text);
 }
 
 search // тип string
 return text.includes(search);
}
```

```ts
interface A { a: number }
interface B { b: number }

function pickAB(ab: A | B) {
  
 if ('a' in ab) {
  ab // тип A
 } else {
  ab // тип B
 }
 
 ab // тип A | B
}
```

3. Добавление к ним явного тега:

```ts
interface UploadEvent { type: 'upload'; filename: string; contents: string }
interface DownloadEvent { type: 'download'; filename: string; }
type AppEvent = UploadEvent | DownloadEvent;

function handleEvent(e: AppEvent) {
     switch (e.type) {
         case 'download':
            console.log(e); // тип DownloadEvent
         break;
         case 'upload':
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
 return 'value' in el;
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
const members = ['Janet', 'Michael'].map(
 who => jackson5.find(n => n === who)
).filter(who => who !== undefined); // тип (string | undefined)[]
```

Однако он одобрит использование защиты типа:

```ts
function isDefined<T>(x: T | undefined): x is T {
 return x !== undefined;
}

const members = ['Janet', 'Michael'].map(
 who => jackson5.find(n => n === who)
).filter(isDefined); // тип string[]
```
