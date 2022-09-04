# React Hooks

## Table of Contents
- [What are Hooks?](#what-are-hooks)
- [Basic Hooks](#basic-hooks)
  - [`useState()`](#usestate)
  - [`useEffect()`](#useeffect)
  - [`useContext()`](#usecontext)
- [Additional Hooks](#additional-hooks)
  - [`useRef()`](#useref)
  - [`useReducer()`](#usereducer)
  - [`useMemo()`](#usememo)
  - [`useCallbac()`](#usecallback)
  - [`useLayoutEffect()`](#uselayouteffect)
  - [`useDebugValue()`](#usedebugvalue)
  - [`useId()`](#useid)
  - [`useTransition()`](#usetransition)
- [Creating Custom Hooks](#creating-custom-hooks)
  - [Example 1 - Toggle](#example-1---toggle)
  - [Example 2 - Controlled Inputs in Forms](#example-2---controlled-inputs-in-forms)

## What are Hooks?
- **Hooks are functions for function components that "hook into" React's state and lifecycle features, which was only possible in class components until React version 16.8.**
- Hooks allow us to write code that is shorter and easier to understand, and also reusable.
- Notes
  - Hooks are functions that are prefixed with "use".
  - Hooks should only be called at the top-level of a functional component.
    - Except for when building your own custom hooks.
### Why do Hooks exist?
- In the past, stateful logic was tightly coupled to a class-based component.
  - Therefore, in order to work with reactive data, you needed to create a class.
  - This resulted into a complex tree of nested components.
  - Sharing any logic required some frustrating solutions like higher ordered components and render props (patterns where components are passed in as arguments to other components).

## Basic Hooks
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
- Syntax: `useEffect(() => {}, [])`
- **Runs after every single render (including the very first render) of the component by default.**
- Commonly used for getting data from an API, save something to a database, manipulate the DOM.
- Notes
  - Think of `useEffect()` as `componentDidMount`, `componentDidUpdate`, and `comopnentWillUnmount` combined.
    - Functional components do not have access to React class lifecycle methods.
  - Class-based components accept a second callback to be executed after manipulating state. However, function-based components do not.
    - Instead, to execute a side effect after rendering, declare it in the component body with `useEffect()`.
- **IMPORTANT**
  - Since `useEffect()` runs any time there is a re-render, changing state in it will result in an endless re-render.
  - **Therefore, `useEffect()` accepts a second argument in the form of an array, which consists of dependencies.**
    - Dependencies can be any number of pieces of data or things in the state.
    - *`useEffect()` will only run if they change.*
- **The function that you may choose to return in the `useEffect` callback will be called when the component is destroyed.**
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
### `useContext()`
- Syntax: `useContext(contextObject)`.
  - Accepts a Context object (created by `React.createContext`) and returns the current context value for that Context.
- `useContext` allows us to work with React's Context API.
- Steps
  - First, use the Context Provider to scope the state for a particular component (since each component can have be different) by passing down the state as an argument for `value`.
    - Now, any child component can inherit that value while avoiding props drilling.
  - Use the `useContext` hook access or consume the value form the Context Provider, which can be many levels higher in the component tree.
- Notes
  - `useContext` is a cleaner replacement for `<Context.Consumer></Context.Consumer>`.
#### Example
- **Setting Up Context API**
```js
import { createContext } from "react";
import MoodEmoji from "MoodEmoji.js";

const moods = {
  happy: "😀",
  sad: "😔"
}

// Create a Context.
const MoodContext = createContext(moods);

function App(props) {
  return (
    <MoodContext.Provider value={moods.happy}>
      <MoodEmoji />
    </MoodContext.Provider>
  )
}
```
- **Using the `useContext` Hook**
```js
function MoodEmoji() {
  // Consume context value from the nearest parent provider.
  const mood = useContext(MoodContext);
  return <p>{ mood }</p>
}

export default MoodEmoji;
```
- Refer [here](https://github.com/Kakamotobi/Learned/blob/main/React/Context-API.md#hooks-based-approach).

## Additional Hooks
### `useRef()`
- Syntax: `useRef(initialValue)`
- **Allows us to create a mutable object that will keep the same reference between renders.**
- Similar to `useState` in that the value is mutable. However, `useRef` does not re-render the component when the value changes.
- Use Case
  - Grab HTML elements from the DOM, use that reference to call native DOM APIs (Ex: `click`, which programmatically clicks the button).
#### Example
```js
import { useRef } from "react";

function App() {
  const myBtn = useRef(null);
  
  const handleClick = () => {
    myBtn.current.click();
  }
  
  return (
    <button ref={myBtn}></button>
  )
}
```
### `useReducer()`
- Syntax: `const [state, dispatch] = useReducer(reducer, initialState, init)`.
  - `dispatch`: a function that can dispatch an action.
    - action: an object that has a `type` and an optional `payload` for data.
      - Actions trigger the `reducer` function.
  - `reducer`: a function where you define what actions lead to what changes in the state.
    - Receives the value of the current state and an `action` to compute the next value of the state.
  - `initialState`: the initial value of the state.
  - `init`: function used to establish the initial state.
- `useReducer` is similar to `useState`.
  - It is just a different way (Redux pattern) to manage state.
- **Instead of updating the state directly, you dispatch actions that go to a reducer function, which then determines how to compute the next state.**
#### Why use `useReducer`?
- It helps us manage complexity as the code grows (the Redux pattern).
- Use along with Context API for state management.
#### Examples
##### Example 1
```js
import { useReducer } from "react";

function reducer(currState, action) {
  // Take the current state and return a new version of that state object based off of the action that has been passed in.
  if (action.type === "INCREMENT") return currState + 1;
  if (action.type === "DECREMENT") return currState - 1;
  if (action.type === "RESET") return 0;
  
  // Convention in Redux
  // switch (action.type) {
    // case "INCREMENT":
      // return currState + 1;
    // case "DECREMENT":
      // return currState - 1;
    // case "RESET":
      // return 0;
  // }
}

function App() {
  const [state, dispatch] = useReducer(reducer, 0);
  
  return (
    <>
      Count: {state}
      <button onClick={() => dispatch({type: "INCREMENT"})}>+</button>
      <button onClick={() => dispatch({type: "DECREMENT"})}>-</button>
      <button onClick={() => dispatch({type: "RESET"})}>Reset</button>
    </>
  )
}
```
##### Example 2
```js
import { useReducer } from "react";

const initialState = { count: 0 };

function reducer(currState, action) {
  if (action.type === "INCREMENT") return { count: currState.count + action.amount };
  if (action.type === "DECREMENT") return { count: currState.count - action.amount };
  if (action.type === "RESET") return { count: 0 };
  
  // switch(action.type) {
    // case "INCREMENT":
      // return { count: currState.count + action.amount };
    // case "DECREMENT":
      // return { count: currState.count - action.amount };
    // case "RESET":
      // return { count: 0 };
  // }
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

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
### `useMemo()`
- Syntax: `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`.
- **Helps us optimize computation costs for improved performance by memoizing/caching the return values of a function call.**
- Use only for expensive calculations; not for everything.
#### Example
```js
import { useMemo } from "react";

function App() {
  const [count, setCount] = useState(0);
  
  const expensiveCount = useMemo(() => {
    return count ** 2;
  }, [count]);
  
  return <></>;
}
```
### `useCallback()`
- Syntax: `const memoizedCallback = useCallback(() => { doSomething(a, b) }, [a, b])`.
- **Memoize an entire function.**
- When you define a function in a component, a new function object is created each time the component rerenders.
  - This is usually not a big issue for performance.
  - However, it may be a good idea to memoize the function in some cases.
- Use Case:
  - When you pass down the same function to multiple child components. By wrapping the function with `useCallback`, unnecessary re-renders of the child components can be prevented (since the function itself will always be the same).
#### Example
```js
import { useCallback } from "react";

function App() {
  const [count, setCount] = useState(0);
  
  const showCount = useCallback(() => {
    alert(`Count: ${count}`);
  });
  
  return <><SomeChild handler={showCount}/></>
}
```
### `useLayoutEffect()`
- It works just like the `useEffect` hook but the callback will run after rendering a component but before the actual updates have been painted to the screen.
  - i.e. **React will wait for your code to finish running before it updates the UI.**
#### Example
```js
function App() {
  const myBtn = useRef(null);
  
  useLayoutEffect(() => {
    const rect = myBtn.current.getBoundingClientRect();
    
    console.log(box.height);
  });
  
  return <><button ref={myBtn}></button></>
}
```
### `useDebugValue()`
- **Allows us to display a label for custom hooks in React DevTools.**
#### Example
```js
import { useDebugValue } from "react";
import { useDisplayName } from "./myCustomHooks.js";

function App() {
  const displayName = useDisplayName();
  
  return <button>{displayName}</button>
}
```
```js
export function useDisplayName() {
  const [displayName, setDisplayName] = useState();
  
  useEffect(() => {
    const data = fetchFromDatabase(props.userId);
    setDisplayName(data.displayName);
  }, []);
  
  useDebugValue(displayName ?? "loading...");
  
  return displayName;
}
```
### `useId()`
- Syntax: `const id = useId()`.
- **Generates unique IDs that are stable across the server and client.**
- *It is NOT for generating keys in a list.*
  - Keys should be generated from your data.
#### Example
```js
import { useId } from "react";

function Box() {
  const id = useId();
  
  return (
    <>
      <label htmlFor={id}>Name</label>
      <input id={id} type="text" name="name" />
    </>
  );
};
```
### `useTransition()`
- Syntax: `const [isPending, startTransition] = useTransition()`.
  - `startTransition` marks udpates in the provided callback as transitions.
  - `isPending` indicates when a transition is active to show a pending state.
- **Returns a stateful value for the pending state of the transition, and a function to start it.**
#### Example
```js
import { useTransition } from "react";
import Spinner from "./Spinner.js";

function App() {
  const [isPending, startTransition] = useTransition();
  const [count, setCount] = useState(0);
  
  function handleClick() {
    startTransition(() => {
      setCount(c => c + 1);
    })
  }

  return (
    <div>
      {isPending && <Spinner />}
      <button onClick={handleClick}>{count}</button>
    </div>
  );
}
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
[10 React Hooks Explained // Plus Build your own from Scratch - YouTube](https://www.youtube.com/watch?v=TNhaISOUy6Q&ab_channel=Fireship)  
