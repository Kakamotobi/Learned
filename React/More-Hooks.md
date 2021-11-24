# More Hooks
- Variants of the basic hooks, or only needed for specific edge cases.

## Table of Contents
- [`useReducer()`](#userreducer)

## `useReducer()`
> An alternative to `useState()`.

> Accepts a reducer of type `(state, action) => newState`, and returns the current state paired with a `dispatch` method.
  - `(state, action) => newState`
    - In React, the two values provided to a reducer function are:
      - The current state.
      - An action that (may) update the state.
        - Ex: `{type: ADD_TODO, task: "Walk The Cat"}`
      - Example
        ```js
        function reducer(state, action) { // switch statement to determine "how" to update the state }
        ```
- **Initializes and returns a state and a function called 'dispatch' that uses a reducer function that we passed in to determine how to update the state.**
- **Use along with Context API for state management.**

### Syntax
```js
const [state, dispatch] = useReducer(reducer, initialArg, init);
```
- `reducer`
  - Your reducer function.
- `initialArg`
  - Initial version of the state.
  - Ex: `{ state1: "" }`
- `init`
  - Function used to establish the initial state.
### Example
```js
// Counter.js

import React, { useReducer } from "react";

// Reducer Function
function countReducer(state, action) {
  // Take the current state and return a new version of that state object based off of the action that has been passed in.
  if (action.type === "INCREMENT") return { count: state.count + action.amount };
  if (action.type === "DECREMENT") return { count: state.count - action.amount };
  if (action.type === "RESET") return { count: 0 };
  
  // Convention in Redux
  // switch(action.type) {
    // case "INCREMENT":
      // return { count: state.count + action.amount };
    // case "DECREMENT":
      // return { count: state.count - action.amount };
    // case "RESET":
      // return { count: 0 };
  // }
  }
}

function Counter() {
  const [state, dispatch] = useReducer(countReducer, {count: 0});

  return (
    <div>
      <h1>{state.count}</h1>
      <button onClick={() => dispatch({type: "INCREMENT", amount: 1})}>Add 1</button>
      <button onClick={() => dispatch({type: "INCREMENT", amount: 5})}>Add 5</button>
      <button onClick={() => dispatch({type: "DECREMENT", amount: 1})}>Subtract 1</button>
      <button onClick={() => dispatch({type: "RESET"})}>Reset</button>
    </div>
  );
}

export default Counter;
```

## Reference
[useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer)
