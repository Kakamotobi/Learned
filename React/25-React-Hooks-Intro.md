# React Hooks Intro

## Table of Contents
- [What are Hooks](#what-are-hooks)
- [Hooks](#hooks)
  - [`useState()`](#usestate)
  - [`useEffect()`](#useeffect)
- [Creating Custom Hooks](#creating-custom-hooks)
  - [Example 1 - Toggle](#example-1---toggle)
  - [Example 2 - Controlled Inputs in Forms](#example-2---controlled-inputs-in-forms)

## What are Hooks
- Hooks allow us to use state and other React features without writing a class.
- Allows us to write functional components that have all the features of a class-based component.
- Benefit:
  - Write code that is shorter and easier to understand, and also reusable.

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
### `useEffect()`
- Syntax: `useEffect(() => {})`
- **Runs after every single render (including the very first render) by default.**
- Commonly used for getting data from an API, save something to a database, manipulate the DOM.
- Notes
  - Think of `useEffect()` as `componentDidMount`, `componentDidUpdate`, and `comopnentWillUnmount` combined.
    - Functional components do not have access to React class lifecycle methods.
  - Class-based comopnents accept a second callback to be executed after manipulating state. However, function-based components do not.
    - Instead, to execute a side effect after rendering, declare it in the comopnent body with `useEffect()`.
- **IMPORTANT**
  - Since `useEffect()` runs any time there is a re-render, changing state in it will result in an endless re-render.
  - Therefore, `useEffect()` accepts a second argument in the form of an array, which consists dependencies.
    - Dependencies can be any number of pieces of data or things in the state.
    - `useEffect()` will only run if they change.
#### Example 1 - `useEffect()` with DOM Manipulation
```js
// Clicker.js

import React, { useState, useEffect } from "react";

function Clicker() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <button onClick={() => setCount(count+1)}>Click Me</button>
  )
}

export default Clicker;
```
#### Example 2 - `useEffect()` with API Call
```js
import React, { useState, useEffect } from "react";
import axios from "axios";

function SWMovies() {
  const [number, setNumber] = useState(1);
  const [movie, setMovie] = useState("");

  useEffect(() => {
    async function getData() {
      const res = await axios.get(`https://swapi.dev/api/films/${number}/`);
      setMovie(res.data);
    }
    getData();
  }, [number]);

  return (
    <div>
      <h1>Pick A Movie</h1>
      <h4>{movie.title}</h4>
      <p>{movie.opening_crawl}</p>
      <select value={number} onChange={evt => setNumber(evt.target.value)}>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
      </select>
    </div>
  );
}

export default SWMovies;
```

## Creating Custom Hooks
### Example 1 - Toggle
#### Before Using Custom Hook
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
#### After Using Custom Hook with `useState()`
- Moved a lot of functionality from the component to a custom hook.
- Cleaned up the component significantly and made a reusable piece of stateful logic for anything to do with toggling.
```js
// useToggle.js

import { useState } from "react";

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
```js
// Toggler.js

import React from "react";
import useToggle from "./hooks/useToggle.js";

function Toggler() {
  const [isHappy, toggleIsHappy] = useToggle(true);
  const [isHeartbroken, toggleIsHeartbroken] = useToggle(false);
  
  return (
    <div>
      <h1 onClick={toggleIsHappy}>{isHappy ? "Happy!" : "Sad!"}</h1>
      <h1 onClick={toggleIsHeartbroken}>{isHeartbroken ? "Heart Broken!" : "Heart Not Broken!"}</h1>
    </div>
  );
}

export default Toggler;
```
### Example 2 - Controlled Inputs in Forms
#### Before Using Custom Hooks
```js
// Form.js

import React, { useState } from "react";

function Form() {
  const [email, setEmail] = useState("");
  
  const handleChange = (evt) => {
    setEmail(evt.target.value);
  }

  return (
    <div>
      <h1>The value is... {email}</h1>
      <input type="text" value={email} onChange={handleChange} />
      <button onClick={() => setEmail("")}>Submit</button>
    </div>
  );
}

export default Form;
```
#### After Using Custom Hooks with `useState()` 
```js
// useInputState.js

import { useState } from "react";

function useInputState(initialVal) {
  const [value, setValue] = useState(initialVal);
  
  const handleChange = (evt) => {
    setValue(evt.target.value);
  }
  const reset = () => {
    setValue("");
  }
  
  // Return three items in an array
  return [value, handleChange, reset];
}

export default useInputState;
```
```js
// Form.js

import React from "react";
import useInputState from "./hooks/useInputState.js";

function Form() {
  const [email, updateEmail, resetEmail] = useInputState("");

  return (
    <div>
      <h1>The value is... {email}</h1>
      <input type="text" value={email} onChange={updateEmail} />
      <button onClick={resetEmail}>Submit</button>
    </div>
  );
}

export default Form;
```
## Reference
[Introducing Hooks - React](https://reactjs.org/docs/hooks-intro.html)
