# Как начать писать на React

> Материалы лекции «Как начать писать на React» от @denysdovhan

Лекция нацелена на людей, которые только слышали о React и хотят больше о нем узнать. В ней мы объясним как начать и куда стоит копать, чтобы разобраться в этой библиотеке.

![YouTube](https://dzwonsemrish7.cloudfront.net/items/0u0g2n12473g1f2R0N0S/Image%202018-06-19%20at%205.38.56%20PM.png?v=fb4a5686)

## Лектор

**Денис Довгань**

Software Consultant, ведет курс [DogeCodes о разработке на React][dogecodes], спикер крупных конференций и митапов (KharkivJS, OdessaJS, Rolling Scopes Conference в Минске), организатор своих воркшопов и конференций — [NodeSchool][nodeschool], [ChernivtsiJS][chernivtsijs].

[**Twitter**][denysdovhan-twitter] • [**GitHub**][denysdovhan-github]

## Ссылки

* [React - A JavaScript library for building user interfaces](https://reactjs.org/) — сайт React.
* [All the fundamental React.js concepts, jammed into this single Medium article](https://medium.freecodecamp.org/all-the-fundamental-react-js-concepts-jammed-into-this-single-medium-article-c83f9b53eac2) — фундаментальные концепции, которые служат основой для React.
* [Pete Hunt: React: Rethinking best practices -- JSConf EU 2013](https://www.youtube.com/watch?v=x7cQ3mrcKaY) — доклад о том, как и зачем создавался React
* [Didact](https://github.com/hexacta/didact) — руководство о том, как написать React своими руками
* [State and Lifecycle - React](https://reactjs.org/docs/state-and-lifecycle.html) — раздел документации React о состоянии и жизненном цыкле компонентов.
* [Handling Events - React](https://reactjs.org/docs/handling-events.html) — раздел из документации React о работе с событиями.
* [React lifecycle methods diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) — диаграма со сравнением методов жизненого цыкла React-компонентов.
* [@angular/core vs angular vs react vs vue](http://www.npmtrends.com/@angular/core-vs-angular-vs-react-vs-vue) — сравнение популярности Angular, Angular.js, React и Vue на npm trends.
* [Flux | Application Architecture for Building User Interfaces](https://facebook.github.io/flux/) — сайт с описанием Flux-архитектуры. 
* [Redux](https://redux.js.org/) — популярная библиотека для управления состоянием в React-приложениях.
* [Getting Started with Redux from @dan_abramov on @eggheadio](https://egghead.io/courses/getting-started-with-redux) — курс об основах Redux от его создателя.
* [Building React Applications with Idiomatic Redux from @dan_abramov on @eggheadio](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) — курс о лучших практиках разработке приложения с React и Redux.  
* [MobX](https://mobx.js.org/) — популярная библиотека для реактивного управления состоянием.
* [Focal](https://github.com/grammarly/focal) — библиотека для реактивного управления состоянием от разработчиков из Grammarly.
* [Интенсив по React | Doge Codes](http://doge.codes/) — интенсив от Дениса о разработке на React.

## Демо

Код из примеров, которые были на лекции:

### Простой компонент

```js
import React from 'react';

const App = ({ name }) => <h1>Hello, {name}!</h1>;

// или

function App({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// или

class App extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### Часы

```js
import React from 'react';

class App extends React.Component {
  state = { date: new Date() };

  componentDidMount() {
    this.timer = setInterval(() => this.tick(), 1000);
   }

  componentWillUnmount() {
    clearInterval(this.timer);
  }

  tick() {
    this.setState({ date: new Date() });
  }

  render() {
    return (
      <div>
        <h2>Time: {this.state.date.toLocaleString()}</h2>
      </div>
    );
  }
}
```

## [Простое To–do–приложение](https://codesandbox.io/s/opk4283kz)

Файл `index.js`:

```js
import React from "react";
import { render } from "react-dom";
import Input from "./Input";
import Todo from "./Todo";

class App extends React.Component {
  state = {
    input: "",
    todos: []
  };

  handleInputChange = event => {
    if (event.target.value === "") return;

    this.setState({
      input: event.target.value
    });
  };

  handleMarkAsDone = id => event => {
    this.setState(prevState => ({
      todos: prevState.todos.map(
        todo =>
          todo.id === id
            ? {
                ...todo,
                done: !todo.done
              }
            : todo
      )
    }));
  };

  handleAddTodo = event => {
    event.preventDefault();

    this.setState(prevState => ({
      input: "",
      todos: [
        ...prevState.todos,
        {
          id: Date.now(),
          done: false,
          text: prevState.input
        }
      ]
    }));
  };

  handleRemoveTodo = id => event => {
    this.setState(prevState => ({
      todos: prevState.todos.filter(todo => todo.id !== id)
    }));
  };

  render() {
    const { todos, input } = this.state;

    return (
      <div>
        <h1>Simple React To-do</h1>
        <Input
          value={input}
          onChange={this.handleInputChange}
          onSubmit={this.handleAddTodo}
        />
        <ul>
          {todos.map(todo => (
            <Todo
              {...todo}
              onMarkAsDone={this.handleMarkAsDone}
              onDelete={this.handleRemoveTodo}
            >
              {todo.text}
            </Todo>
          ))}
        </ul>
      </div>
    );
  }
}

render(<App />, document.getElementById("root"));
```

Файл `Input.js`:

```js
import React from "react";

const Input = ({ value, onChange, onSubmit }) => (
  <form onSubmit={onSubmit}>
    <input type="text" value={value} onChange={onChange} />
    <button type="submit">Add</button>
  </form>
);

export default Input;

```

Файл `Todo.js`:

```js
import React from "react";

const Todo = ({ done, id, text, onMarkAsDone, onDelete, children }) => (
  <li>
    <span
      style={{
        textDecoration: done ? "line-through" : "none"
      }}
      onClick={onMarkAsDone(id)}
    >
      {children}
    </span>{" "}
    <button onClick={onDelete(id)}>x</button>
  </li>
);

export default Todo;

```

<!-- Referenses -->

[dogecodes]: http://doge.codes/
[nodeschool]: https://nodeschool.io/chernivtsi/
[chernivtsijs]: https://chernivtsi.js.org/
[denysdovhan-twitter]: https://twitter.com/denysdovhan
[denysdovhan-github]: https://github.com/denysdovhan
