# React State Patterns

## Table of Contents
- [Updating Existing State](#updating-existing-state)
- [Mutable Data Structures in State](#mutable-data-structures-in-state)
- [Rules of Thumb for Designing State](#rules-of-thumb-for-designing-state)
  - [Minimize Your State](#minimize-your-state)
  - [State Should Live on the Parent](#state-should-live-on-the-parent)
  - [Example](#example)

## Updating Existing State
- Setting state using existing values in state.
- **If a call to `setState()` depends on the current state, the safest thing is to use the alternate "callback form".**
  - Syntax: `this.setState(callback)`
    - Ex: `this.setState(currState => ({ count: currState.count + 1 }))`
  - ***Instead of passing an object, pass in a callback with the current state as a parameter.***
  - ***The callback should return an object representing the new state.***
### Functional `setState`
- We can describe our state updates abstractly as separate functions.
- This pattern comes up a lot in Redux.
- Example
  ```js
  function incrementCounter(prevState) {
    return { count: prevState.count + 1 };
  }
  
  this.setState(incrementCounter);
  ```
### Example
```js
// ScoreKeeper.js

class ScoreKeeper extends Component {
  constructor(props) {
    super(props);
    this.state = { score: 0 };
    this.singleKill = this.singleKill.bind(this);
    this.tripleKill = this.tripleKill.bind(this);
  }
  
  // One Approach
  singleKill() {
    // Take the current value of this.state.score and add 1.
    this.setState((currState) => ({ score: currState.score + 1 }));
  }
  
  // Functional setState Approach
  incrementScore(currState) {
    return { score: currState.score + 1 };
  }
  
  tripleKill() {
    this.setState(this.incrementScore);
    this.setState(this.incrementScore);
    this.setState(this.incrementScore);
  }
  
  render() {
    return (
      <div>
        <h1>Score is: {this.state.score}</h1>
        <button onClick={this.singleKill}>Single Kill!</button>
      </div>
    )
  }
}
```

## Mutable Data Structures in State
- Objects, arrays, arrays of objects, etc.
- *Mutating nested data structures in our state can cause problems with React.*
- ***Therefore, always make a new copy of the data structure in question, and then call `this.setState` with the new copy.***
  - Pure functions such as `.map()`, `.filter()`, `.reduce()`, and the spread operator will be used often.
### Example
```js
this.state = {
  todos: [
    {task: "do the dishes", done: false, id: 1 }
    {task: "vacuum the floor", done: false, id: 2 }
  ]
};
```
```js
completeTodo(id) {
  const newTodos = this.state.todos.map(todo => {
    if (todo.id === id) {
      // make a copy of the todo object with done set to true.
      return { ...todo, done: true };
    }
    return todo; // old todos can pass through.
  });

  this.setState({
    todos: newTodos // setState to the new array.
  })
}
```

## Rules of Thumb for Designing State
### Minimize Your State
- Put as little data in state as possible.
- Litmus Test
  - Does x change? if not, x should not be part of state. It should be a prop.
  - Is x already captured by some other value y in state or props? If so, derive it from there instead.
### State Should Live on the Parent
- We want to support the "downward data flow" philosophy of React.
- This makes debugging easier, because the state is centralized.
- Example
  - `TodoList` is a smart parent with lots of methods, while the individual Todo ietms are just `<li>` tags with some text and styling.
  ```js
  // TodoList.js
  
  class TodoList extends Component {
    constructor(props) {
      super(props);
      this.state = {
        todos: [
          { task: "do the dishes", done: false, id: 1 }
          { task: "vacuum the floor", done: false, id: 2 }
        ]
      };
    }
    
    // ... other methods ... //
    
    render() {
      return (
        <ul>
          {this.state.todos.map(t => <Todo {...t} />)}
        </ul>
      );
    }    
  }
  ```
### Example
```js
// Lottery.js

import React, { Component } from "react";
import LottoBall from "./LottoBall.js";
import "./Lottery.css";

class Lottery extends Component {
	static defaultProps = {
		title: "Lotto",
		numBalls: 6,
		maxNum: 40,
	};

	constructor(props) {
		super(props);
		this.state = {
			// Create numBalls amount of indices in an array.
			nums: Array.from({ length: this.props.numBalls }),
		};
		this.handleClick = this.handleClick.bind(this);
	}

	generate() {
		this.setState((currState) => ({
			// Fill array with random numbers.
			nums: currState.nums.map(
				(n) => Math.floor(Math.random() * this.props.maxNum) + 1
			),
		}));
	}

	handleClick() {
		this.generate();
	}

	render() {
		return (
			<section className="Lottery">
				<h1 className="Lottery__title">{this.props.title}</h1>
				<div className="Lottery__LottoBall-container">
					{this.state.nums.map((n) => (
						<LottoBall num={n} />
					))}
				</div>
				<button className="Lottery__btn" onClick={this.handleClick}>Generate</button>
			</section>
		);
	}
}

export default Lottery;
```
```js
// LottoBall.js

import React, { Component } from "react";
import "./LottoBall.css";

class LottoBall extends Component {
	render() {
		return <div className="LottoBall">{this.props.num}</div>;
	}
}

export default LottoBall;
```
