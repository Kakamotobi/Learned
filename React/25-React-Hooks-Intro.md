# React Hooks Intro

## Table of Contents
- [What are Hooks](#what-are-hooks)
- [Hooks](#hooks)
  - [`useState()`](#usestate)

## What are Hooks
- Hooks allow us to use state and other React features without writing a class.
- Allows us to write functional components that have all the features of a class-based component.
- Benefit:
  - Write code that is shorter and easier to understand, and also reusable.
  - Allows 

## Hooks
### `useState()`
- Syntax: `const [stateName, setState] = useState(initStateVal)`
- **`useState()` returns an array with two different pieces.**
  - Reference to the state itself.
  - A function to update that piece of state.
#### Example
```js
import React, { useState } from "react";

function CounterHooks() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>The Count Is: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Add 1</button>
    </div>
  );
}

export default CounterHooks;
```


## Reference
[Introducing Hooks - React](https://reactjs.org/docs/hooks-intro.html)
