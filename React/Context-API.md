# Context API

## Table of Contents
- [What is Context API](#what-is-context-api)
- [How to Create Context](#how-to-create-context)

## What is Context API
> Context provides a way to pass data through the component tree without having to pass props down manually at every level.

> Context is primarily used when some data needs to be accessible by *many* components at different nesting levels. Apply it sparingly because it makes component reuse more difficult.
- Context can be used in combination with hooks (useContext, useReducer) to create a centralized data store (Context) where all of the state for the entire application is managed - just like Redux.

## How to Create Context
- Each Context has two pieces: the Provider and the Consumer.

1. Create Context object.
2. Create a Provider component that returns a wrapper using that Context.Provider.
3. Give the Provider a single value for consuming components to access.
### `React.createContext()`
- Defines a new Context object.
### `Context.Provider`
- Upon creating a Context, we get a Provider React component.
- Returns a wrapper using the Context.Provider.
  - Components wrapped inside this Provider has access to the value given to the Provider.
- It allows consuming components to subscribe to Context changes.
- It accepts a **`value`** prop to be passed to consuming components that are descendants of this Provider.
- Note:
  - *All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes.*
  - *The propagation from Provider to its descendant consumers (including .contextType and useContext) is not subject to the shouldComponentUpdate method, so the consumer is updated even when an ancestor component skips an update.*
### Example
```js
// ThemeContext.js

import React, { Component, createContext } from "react";

const ThemeContext = createContext();

class ThemeProvider extends Component {
	constructor(props) {
		super(props);
		this.state = {
			isDarkMode: true,
		};
	}

	render() {
		return (
			<ThemeContext.Provider value={{ ...this.state }}>
				{this.props.children}
			</ThemeContext.Provider>
		);
	}
}

// Export ModeContext because we need access to it to consume it in other components.
export { ThemeContext, ThemeProvider };
```
```js
// App.js

import { ThemeContext, ThemeProvider } from "./contexts/ThemeContext.js";
import PageContent from "./PageContent.js";
import Navbar from "./Navbar.js";
import Form from "./Form.js";

function App() {
  return (
    <ThemeProvider>
      <PageContent>
        <Navbar />
        <Form />
      </PageContent>
    </ThemeProvider>
  )
}

export default App;
```


## Reference
[Context - React](https://reactjs.org/docs/context.html#gatsby-focus-wrapper)  
[`createContext()` and `Context.Provider`](https://reactjs.org/docs/context.html#reactcreatecontext)
