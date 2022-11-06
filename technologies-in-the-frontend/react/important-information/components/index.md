# Компоненты

## Названия компонентов

Всегда называйте компоненты с заглавной буквы. Если компонент начинается с маленькой буквы, React принимает его за DOM-тег.
Например, `<div />` это div-тег из HTML, а `<Welcome />` это уже наш компонент Welcome, который должен быть в области видимости

## Извлечение компонентов

Если вы не уверены, извлекать компонент или нет, вот простое правило.
Если какая-то часть интерфейса многократно в нём повторяется (Button, Panel, Avatar) или сама по себе достаточно сложная
(App, FeedStory, Comment), имеет смысл её вынести в независимый компонент.

## this

При обращении к this в JSX-колбэках необходимо учитывать, что методы класса в JavaScript по умолчанию не привязаны к контексту.
Если вы забудете привязать метод `this.handleClick` и передать его в `onClick`, значение this будет undefined в момент вызова функции.

Дело не в работе React, это часть того, как работают функции в JavaScript. В JavaScript эти два фрагмента кода не равнозначны:

```js
obj.method();

var method = obj.method;
method();
```

Привязка гарантирует, что второй фрагмент будет работать так же, как и первый.

В React, как правило, привязывать нужно только те методы, которые вы хотите передать другим компонентам.
Например, `<button onClick={this.handleClick}>` передаёт `this.handleClick`, поэтому его нужно привязать.
Впрочем, метод render и методы жизненного цикла привязывать не обязательно, так как мы не передаём их в другие компоненты.
Пример:

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };

    // Эта привязка обязательна для работы `this` в колбэке.
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState((prevState) => ({
      isToggleOn: !prevState.isToggleOn,
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? "Включено" : "Выключено"}
      </button>
    );
  }
}

ReactDOM.render(<Toggle />, document.getElementById("root"));
```

## dangerouslySetInnerHTML

Свойству `innerHTML` в DOM браузера соответствует `dangerouslySetInnerHTML` в React.
Как правило, вставка HTML из кода рискованна, так как можно случайно подвергнуть
ваших пользователей атаке межсайтового скриптинга.
Таким образом, вы можете вставить HTML непосредственно из React используя `dangerouslySetInnerHTML`
и передать объект с ключом `__html`, чтобы напомнить себе, что это небезопасно. Например:

```jsx
function createMarkup() {
  return { __html: "Первый &middot; Второй" };
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```
