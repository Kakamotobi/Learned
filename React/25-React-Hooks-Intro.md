# React Hooks Intro

## Table of Contents
- [What are Hooks](#what-are-hooks)
- [Hooks](#hooks)
  - [`useState()`](#usestate)
- [Creating Custom Hooks](#creating-custom=hooks)

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

## Creating Custom Hooks
### Example - Before Using Custom Hook
```js
// Toggler.js

import React, { useState } from "react";

function Toggler() {
  const [isHappy, setIsHappy] = useState(true);
  const [isHeartbroken, setIsHeartbroken] = useState(false);
  
  const toggleIsHappy = () => {
    setIsHappy(!isHappy);
  };
  
  const toggleIsHeartbroken = () => {
    setIsHeartbroken(!isHeartbroken);
  }
  
  return (
    <div>
      <h1 onClick={toggleIsHappy}>{isHappy ? "Happy!" : "Sad!"}</h1>
      <h1 onClick={toggleIsHeartbroken}>{isHeartbroken ? "Heart Broken!" : "Heart Not Broken!"}</h1>
    </div>
  );
}

export default Toggler;
```
### Example - Custom Hook using `useState()`
```js
// useToggle.js

import {useState} from "react";

function useToggle(initialVal = false) {
  // Call useState, "reserve piece of state".
  const [state, setState] = useState(initialVal);
  
  // Define function to update state.
  const toggle = () => {
    setState(!state);
  };
  
  // Return piece of state AND a function to toggle it.
  return [state, toggle];
}

export default useToggle;
```
- Move a lot of functionality from the component to a custom hook.
```js
// Toggler.js

import React, { useState } from "react";
import useToggle from "./hooks/useToggle.js";

function Toggler() {
  const [isHappy, toggleIsHappy] = useToggle(true);
  const [isHeartbroken, togleIsHeartbroken] = useToggle(false);
  
  return (
    <div>
      <h1 onClick={toggleIsHappy}>{isHappy ? "Happy!" : "Sad!"}</h1>
      <h1 onClick={toggleIsHeartbroken}>{isHeartbroken ? "Heart Broken!" : "Heart Not Broken!"}</h1>
    </div>
  );
}

export default Toggler;
```

## Reference
[Introducing Hooks - React](https://reactjs.org/docs/hooks-intro.html)
