# Классы

## Активно используйте принцип SOLID (с умом!)

- Принцип единственной ответственности
- Принцип открытости/закрытости
- Принцип подстановки Барбары Лисков
- Принцип разделения интерфейса
- И принцип инверсии зависимостей

[Подробнее о принципах](solid/index.md)

---

## Используйте геттеры и сеттеры

👎**Плохо:**

```javascript
class BankAccount {
  constructor() {
    this.balance = 1000;
  }
}

const bankAccount = new BankAccount();
bankAccount.balance -= 100;
```

👍**Хорошо:**

```javascript
class BankAccount {
  constructor(balance = 1000) {
    this._balance = balance;
  }

  set balance(amount) {
    if (this.verifyIfAmountCanBeSetted(amount)) {
      this._balance = amount;
    }
  }

  get balance() {
    return this._balance;
  }

  verifyIfAmountCanBeSetted(val) {
    // ...
  }
}

const bankAccount = new BankAccount();
bankAccount.balance -= shoesPrice;
let balance = bankAccount.balance;
```

---

## Используйте метод цепочки

👎**Плохо:**

```javascript
class CarBuilder {
  constructor() {
    this.car = new Car();
  }

  addAutoPilot(autoPilot) {
    this.car.autoPilot = autoPilot;
  }

  addParktronic(parktronic) {
    this.car.parktronic = parktronic;
  }

  addSignaling(signaling) {
    this.car.signaling = signaling;
  }

  updateEngine(engine) {
    this.car.engine = engine;
  }

  build() {
    return this.car;
  }
}

const car = new CarBuilder();
car.addAutoPilot(true);
car.addParktronic(true);
car.updateEngine("V8");
car.build();
```

👍**Хорошо:**

```javascript
class CarBuilder {
  constructor() {
    this.car = new Car();
  }

  addAutoPilot(autoPilot) {
    this.car.autoPilot = autoPilot;
    return this;
  }

  addParktronic(parktronic) {
    this.car.parktronic = parktronic;
    return this;
  }

  addSignaling(signaling) {
    this.car.signaling = signaling;
    return this;
  }

  updateEngine(engine) {
    this.car.engine = engine;
    return this;
  }

  build() {
    return this.car;
  }
}

const car = new Car()
  .addAutoPilot(true)
  .addParktronic(true)
  .updateEngine("V8")
  .build();
```
