# Асинхронность

## Используйте промисы вместо колбеков, там где это возможно

👎**Плохо:**

```javascript
request.get(url, (requestErr, response) => {
  requestErr
    ? console.error(requestErr)
    : fs.writeFile("article.html", response.body, (writeErr) => {
        writeErr ? console.error(writeErr) : console.log("File written");
      });
});
```

👍**Хорошо:**

```javascript
requestPromise
  .get(url)
  .then((response) => fsPromise.writeFile("article.html", response))
  .then(() => {
    console.log("File written");
  })
  .catch((err) => {
    console.error(err);
  });
```

## Используйте Async/Await вместо промисов, там где это возможно

👎**Плохо:**

```javascript
requestPromise
  .get(url)
  .then((response) => fsPromise.writeFile("article.html", response))
  .then(() => {
    console.log("File written");
  })
  .catch((err) => {
    console.error(err);
  });
```

👍**Хорошо:**

```javascript
async function getCleanCodeArticle(url) {
  try {
    const response = await requestPromise.get(url);
    await fsPromise.writeFile("article.html", response);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}
```

## Не игнорируйте отловленные ошибки

👎**Плохо:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

👍**Хорошо:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.error(error);
  notifyUserOfError(error);
  reportErrorToService(error);
}
```
