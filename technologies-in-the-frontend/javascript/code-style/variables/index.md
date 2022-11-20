# Переменные

## Используйте значимые и смысловые имена

👎**Плохо:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

👍**Хорошо:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

## Используйте один вариант именования

👎**Плохо:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

👍**Хорошо:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

## Используйте один и тот же метод для одинаковых типов переменных

👎**Плохо:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

👍**Хорошо:**

```javascript
getUser();
```

## Используйте именованные значения

👎**Плохо:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

👍**Хорошо:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

## Используйте поясняющие переменные

👎**Плохо:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;

saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

👍**Хорошо:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;

const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

## Избегайте бессмысленных названий переменных в маппинге

👎**Плохо:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];

locations.forEach((l) => {
  doSomething(l);
});
```

👍**Хорошо:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];

locations.forEach((location) => {
  doSomething(location);
});
```

## Избегайте мутирования переменных, не принадлежащих аргументам функции

👎**Плохо:**

```javascript
function paintCar(car) {
  car.carColor = "Red";
}
```

👍**Хорошо:**

```javascript
function paintCar(car) {
  const carWithRedColor = { ...car };

  car.color = "Red";

  return carWithRedColor;
}
```

## Используйте условия по умолчанию вместо замыканий или условных выражений

👎**Плохо:**

```javascript
const createFullName = (firstName, lastName) => {
  const fName = firstName || "Yauhen";
  // ...
};
```

👍**Хорошо:**

```javascript
const createFullName = (firstName = "Yauhen", lastName = "Kavalchuk") => {
  // ...
};
```
