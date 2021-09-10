# React State
- Important in making any sort of interactive React application.

## Table of Contents
- [State](#state)
- [React State](#react-state)
  - [Initializing State](#initializing-state)
  - [Setting State](#setting-state)
- [React Events](#react-events)
  - [this and .bind()](#this-and-bind)
  - [React Event Example](#react-event-example)

## State
- **Internal data specific to a component.**
  - **Represents data that can and usually changes over time.**
- In any sufficiently advanced web application, the UI has to be stateful.
- *Some data are liable to change, and we want to keep track of it somewhere.*
  - Ex: logged-in users see a different screen than logged-out users.
  - Ex: clicking "edit profile" opens up a modal (pop-up) window.
  - Ex: accordions, read more.
- The state of the client interface (front-end) is not always directly tied to the state on the server (back-end).
  - The backend doesn't need to know whether the modal is open or not.
- *State is designed to change in response to events.*
  - Ex: in a chess game, at any point in time, the board is in a complex state. Every new move represents a single discrete state change.
### 2 Things that State Tracks
- **UI Logic** - the changing state of the interface.
  - Ex: there is a modal open right now because I'm editing my profile.
- **Business Logic** - the changing state of data itself.
  - Ex: new notification, read or unread messages, etc.

## React State
- **In React, state is an instance attribute on a component.**
- *It's always a JS object (POJO), since you'll want to keep track of several **key/value pairs**.*
  - Ex: `console.log(this.state);` inside of a component will return the key/value pairs. 
### Initializing State
- **State should be initialized as soon as the component is created.**
- So, we set it in the ***constructor function***.
- *If your component is stateless, you can omit the constructor function.*
- ***If you are building a component with state, you need a standard React constructor.***
  - *Hence, need to use class-based component instead of function-based*.
  - `constructor` takes one argument, `props`.
  - Must call **`super(props)`** at the start of the constructor, which "registers" your class as a React Component.
    - `super` is a reference to the constructor of the Component.
#### Example
```js
import React, { Component } from "react";

class Game extends Component {
  constructor(props) {
    super(props);
    this.state = {
      gameOver: false,
      score: 0
    }
  }
  
  render() {
    return (
      <div>
        <h1>Fun Game</h1>
        <p>Your Score Is: {this.state.score}</p>
      </div>
    );
  }
}

export default Game;
```
#### `super()` and `super(props)`
- Need to include when using states.
##### **`super()`**
- **Since we are extending React's Component class, we need to call its constructor through `super()`.**
###### Example
```js
// Ordinary Class

class Component {
  constructor() {
    console.log("Inside the Component Constructor!")
  }
}
```
```js
// Class Using React Component

class Game extends Component {
  constructor() {
    super();
    console.log("Inside the Game Constructor!")
  }
}
```
##### **`super(props)`**
- **If we want access to `this.props` in the constructor, we have to pass `props` to `super()`.**
- We have access to `this.props` in any other methods (ex: `render() {}`) inside of the class except the constructor.
###### Example
```js
// App.js

class App extends Component {
  render() {
    return (
      <div className="App">
        <Demo animal="Bobcat" food="Pineapple" />
      </div>
    )
  }
}

export default App;
```
```js
// Demo.js

class Demo extends Component {
  constructor(props) {
    super(props);
    console.log(this.props);
  }
  render() {
    return <h1>Demo Component!</h1>
  }
}

export default Demo;
```
### Setting State
- Never directly manipulate the State. Instead, use `this,.setState()`.
#### **`this.setState()`**
- The built-in React method of changing a component's state.
> Think of `setState()` as a request rather than an immediate command to update the component. For better perceived performance, React may delay it, and then update several comoponents in a single pass. React does not guarantee that the state changes are applied immediately.
  - Hence, it is important to not directly manipulate the state ourselves. We should go through React.
- There are different ways to use `setState()`.
  - Pass in a callback.
  - Pass in an object with the key/value pairs that we want to change in the State.
    - Ex: `this.setState({ playerName: "Matt", score: 0 })`
    - Takes an object describing the way the state should change.
    - Patches the state object with the changes - keys that haven't been specified don't change.
    - All of this happens asynchronously.
      - The component state will *eventually* update.
      - We're merely asking/requesting React to change the state.
      - React controls when the state will actually change, for performance reasons.
    - The entire component re-renders when their state changes.
    - *Do not call it in the constructor or `render(){}`.*
    - *Can be called in any instance method.*
#### Example
```js
// App.js

class App extends Component {
  render() {
    return (
      <div className="App">
        <Rando maxNum={7} />
      </div>
    )
  }
}

export default App;
```
```js
// Rando.js

class Rando extends Component {
  constructor(props) {
    super(props);
    this.state = { num: 0 };
    this.makeTimer();
  }
  
  makeTimer() {
    setInterval(() => {
      let rand = Math.floor(Math.random() * this.props.maxNum);
      this.setState({ num: rand });
    }, 1000);
  }
  
  render() {
    return <h1>{this.state.num}</h1>
  }
}

export default Rando;
```
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
- Therefore, we need to **`.bind()`** the function/method in the constructor.
  - We're setting the context of the function by binding `this`.
  - ***The value of `this` inside the constructor is of the individual component.***
    - `.bind()` is used to set that context (individual component that has been created).
### React Event Example
```js
// App.js

function App() {
  return(
    <div className="App">
      <Clicker />
    </div>
  )
}

export default App;
```
```js
// Clicker.js

class Clicker extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num: 1,
    };
    this.genRandNum = this.genRandNum.bind(this); // `bind()` the value of `this` to the instance of the individual component.
  }
  
  genRandNum(evt) {
    let randNum = Math.floor(Math.random() * 10) + 1;
    this.setState({ num: randNum });
  }
  
  render() {
    return (
      <div className="Clicker">
        <h1 classname="Clicker-number">Number is: {this.state.num}</h1>
        {this.state.num === 7 ? (
          <h2>You Win!</h2>
        ) : (
          <button class="Clicker-btn" onclick={this.genRandNum}>
            Random Number
          </button>
        )}
      </div>
    );
  }
}

export default Clicker;
```

## State vs. Props
| term | structure | mutable | purpose                                        |
| :--: | :-------: | :-----: | :--------------------------------------------: |
| state| POJO `{}` | yes     | stores changing data relating to the component |
| props| POJO `{}` | no      | stores component configuration information     |

### "State as Props"
- A comon pattern we will see often is a stateful ("smart") parent component passing down its state values as props to stateless ("dumb") child components.
- This idea is generalized in React as "downward data flow".
  - Means that components get simpler as you go down the component hierarchy, and parents tend to be more stateful than their children.
#### Example
```js
class CounterParent extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 5 };
  }
  
  render() {
    return(
      <div>
        <CounterChild count={this.state.count}
      </div>
    )
  }
}
```
