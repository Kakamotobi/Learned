# React State Patterns

## Table of Contents
- [Updating Existing State](#updating-existing-state)
- [Mutable Data Structures in State](#mutable-data-structures-in-state)

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
