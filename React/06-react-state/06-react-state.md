# React State
- Important in making any sort of interactive React application.

## Table of Contents
- [State](#state)
- [React State](#react-state)
  - [Initializing State](#initializing-state)
  - [Setting State](#setting-state)
- [React Events](#react-events)
  - [this and .bind()](#this-and-bind)

## State
- Internal data specific to a component.
  - Represents data that can and usually changes over time.

- In any sufficiently advanced web application, the UI has to be stateful.
- Some data are liable to change, and we want to keep track of it somewhere.
  - Ex: logged-in users see a different screen than logged-out users.
  - Ex: clicking "edit profile" opens up a modal (pop-up) window.
  - Ex: accordions, read more.
- The state of the client interface (front-end) is not always directly tied to the state on the server (back-end).
- *State is designed to change in response to events.*
  - Ex: in a chess game, at any point in time, the board is in a complex state. Every new move represents a single discrete state change.
### What does State Track?
- There are 2 types of things that state on the client/frontend keeps track of:
  - **UI Logic** - the changing state of the interface.
    - Ex: there is a modal open right now because I'm editing my profile.
  - **Business Logic** - the changing state of data itself.
    - Ex: new notification, read or unread messages, etc.

## React State
- In React, state is an instance attribute on a component.
- It's always a JS object (POJO), since you'll want to keep track of several keys/values.
  - Ex: `console.log(this.state);` inside of a component will return the key/value pairs. 
### Initializing State
- State should be initialized as soon as the component is created.
- So, we set it in the constructor function.
- *If your component is stateless, you can omit the constructor function.*
- *If you are buidling a component with state, you need a standard React constructor.*
  - `constructor` takes one argument, `props`.
  - Must call `super(props)` at the start of the constructor, which "registers" your class as a React Component.
    - `super` is a reference to the constructor of the component.
#### Example
```
class ClickCount extends Component {
  constructor(props) {
    super(props);
    this.state = {
      numClicks: 0  // start at zero clicks
    };
  }
}
```
#### `super()` and `super(props)`
##### **`super()`**
  - Making sure that we call the constructor in the base Component class.
###### Example
```
class Component {
  constructor() {
    console.log("Inside the Component Constructor!")
  }
}

class Game extends Component {
  constructor() {
    super();
    console.log("Inside the Game Constructor!")
  }
}
```
##### **`super(props)`**
  - If we want access to `this.props` in the constructor, we have to pass `props` to `super()`.
  - We have access to `this.props` in any other methods (ex: `render() {}`) inside of the class except the constructor.
###### Example
```
<<App.js>>
class App extends Component {
  render() {
    return (
      <div className="App">
        <Demo animal="Bobcat" food="Pineapple" />
      </div>
    )
  }
}

<<Demo.js>>
class Demo extends Component {
  constructor(props) {
    super();
    
  }
  render() {
    return <h1>Demo Component!</h1>
  }
}
```
### Setting State
- Never directly manipulate the State. Instead, use `this,.setState()`.
#### **`this.setState()`**
- The built-in React method of changing a component's state.
> Think of `setState()` as a request rather than an immediate command to update the component. For better perceived performance, React may delay it, and then update several comoponents in a single pass. React does not guarantee that the state changes are applied immediately.
  - Hence, important to not directly manipulate the state ourselves. We should go through React.
- There are different ways to use `setState()`.
  - Pass in a callback.
  - Pass in an object with the key/value pairs that we want to change in the State.
    - Ex: `this.setState({ playerName: "Matt", score: 0 })`
    - Can be called in any instance method except the constructor.
    - Takes an object describing the way the state should change.
    - Patches the state object with the changes - keys that haven't been specified don't change.
    - All of this happens asynchronously.
      - The component state will *eventually* update.
      - We're merely asking/requesting React to change the state.
      - React controls when the state will actually change, for performance reasons.
    - The entire component re-renders when their state changes.
#### Reference
[setState() - React](https://reactjs.org/docs/react-component.html#setstate)

## React Events
- State most commonly changes in direct response to user-invoked event.
- In React, every JSX element has built-in attributes representing every kind of browser event.
  - Ex: `<button onClick={function(evt) {alert("You clicked me!");}}>Click Me!<button>`
### `this` and `.bind()`
- `this` is undefined, and hence, `this.setState()` results an error.
- React calls the callback on an event, but it doesn't call it on our specific instance of the component.
  - The method was called "out of context".
- Therefore, we need to **`.bind()`** the function.
  - We're setting the context of the function by binding `this`.
  - ***The value of `this` inside the constructor is of the individual component.***
#### Example
```
<<App.js>>
import React, {Component} from "react";
import BrokenClick from "./Brokenclick";

class App extends Component {
  render() {
    return (
      <div>
         <BrokenClick />
      </div>
    )
  }
}

export default App;

<<BrokenClick>>
import React, {Component} from "react";

class BrokenClick extends Component {
  constructor(props) {
    super(props);
    this.state = {
      clicked: false;
    }
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick(evt) {
    this.setState({clicked: true});
  }

  render() {
    return (
      <div>
        <h1>{this.state.clicked ? "Clicked!" : "Not Clicked!"}</h1>
        <button onclick={this.handleClick}>Click Me!</button>
      </div>
    )
  }
}
```
