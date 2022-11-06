# Функции

## Функция должна решать только одну задачу

👎**Плохо:**

```javascript
const emailClients = (clients) => {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
};
```

👍**Хорошо:**

```javascript
const emailActiveClients = (clients) => {
  clients.filter(isActiveClient).forEach(email);
};

const isActiveClient = (client) => {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
};
```

## Имена должны описывать что функция делает

👎**Плохо:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();
// It's hard to to tell from the function name what is added
addToDate(date, 1);
```

👍**Хорошо:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

## Не используйте флаги, как аргументы функций

👎**Плохо:**

```javascript
const createFile = (name, temp) => {
  temp ? fs.create(`./temp/${name}`) : fs.create(name);
};
```

👍**Хорошо:**

```javascript
const createFile = (name) => {
  fs.create(name);
};

const createTempFile = (name) => {
  createFile(`./temp/${name}`);
};
```

## Инкапсулируйте условия

👎**Плохо:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

👍**Хорошо:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

## Избегайте негативных условий

👎**Плохо:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

👍**Хорошо:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

## Идеальное количество аргументов для функций два и меньше

👎**Плохо:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

👍**Хорошо:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

## Не переопределяйте глобальные функции

👎**Плохо:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter((elem) => !hash.has(elem));
};
```

👍**Хорошо:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter((elem) => !hash.has(elem));
  }
}
```

## Удаляйте мёртвый код

👎**Плохо:**

```javascript
// Bad
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "test-url");
```

👍**Хорошо:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "test-url");
```
