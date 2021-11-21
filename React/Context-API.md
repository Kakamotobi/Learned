# Context API

## Table of Contents
- [What is Context API](#what-is-context-api)
- [How to Create Context](#how-to-create-context)
- [How to Consume Information from Context](#how-to-consume-information-from-context)
	- [Option 1 (Consuming One Context) - `Class.contextType`](#option-1-consuming-one-context---classcontexttype)
	- [Option 2 (Consuming Multiple Context) - `Context.Consumer`](#option-2-consuming-multiple-context---contextconsumer)
- [Hooks-based Approach](#hooks-based-approach)
	- [useContext() Hook](#usecontext-hook)

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
- It accepts a SINGLE **`value`** prop to be passed to consuming components that are descendants of this Provider.
	- You can pass in states, methods, and any other props (all as a single object).
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
			darkMode: true,
		};

		this.toggleMode = this.toggleMode.bind(this);
	}

	toggleMode() {
		this.setState({ darkMode: !this.state.darkMode });
	}

	render() {
		return (
			<ThemeContext.Provider value={{ ...this.state, toggleMode: this.toggleMode }}>
				{this.props.children}
			</ThemeContext.Provider>
		);
	}
}

export { ThemeContext, ThemeProvider };
```
```js
// App.js

import { ThemeProvider } from "./contexts/ThemeContext.js";
import PageContent from "./PageContent.js";
import Navbar from "./Navbar.js";
import Greet from "./Greet.js";

function App() {
  return (
    <ThemeProvider>
      <PageContent>
        <Navbar />
        <Greet />
      </PageContent>
    </ThemeProvider>
  )
}

export default App;
```

## How to Consume Information from Context
- Import the Context that you created into the consuming component and tell it to use that Context.
### Option 1 (Consuming One Context) - `Class.contextType`
- The contextType property on a class can be assigned a Context object created by `React.createContext()`. 
- *Using this property lets you consume the nearest current value of that Context type using **`this.context`***.
	- *You can reference this in any of the lifecycle methods including the render function.*
- This option only allows you to consume just one Context.
#### Syntax
```js
// Syntax 1
class Myclass extends React.Component {
	render() {
    let value = this.context;
    /* render something based on the value of MyContext */
  }
};

MyClass.contextType = MyContext;

// Syntax 2
class MyClass extends React.Component {
  static contextType = MyContext;
  render() {
    let value = this.context;
    /* render something based on the value */
  }
}
```
#### Example
```js
// Navbar.js

import React, { Component } from "react";
import AppBar from "@mui/material/AppBar";
import Switch from "@mui/material/Switch";
import { ThemeContext } from "./contexts/ThemeContext.js";

class Navbar extends Component {
	// Check to see if this component (Navbar) is nested inside of a ThemeContext.
	static contextType = ThemeContext;
	
	render() {
		const { darkMode, toggleMode } = this.context;
		
		return (
			<AppBar position="static" color={darkMode ? "default" : "primary"}>
				<Switch color="default" onChange={toggleMode} />
			</AppBar>
		);
	}
}

export default Navbar;
```
### Option 2 (Consuming Multiple Context) - `Context.Consumer`
- A React component that lets you subscribe to a Context.
	- Using this component lets you subscribe to a Context within a function component.
- **Requires a function as a child.**
- The value argument passed to the function will be equal to the value prop of the closest Provider for this context above in the tree.
- Context is passed in as a prop through a Higher Order Component to the consuming component.
#### Syntax
```js
<MyContext.Consumer>
  {value => /* render something based on the value of MyContext */}
</MyContext.Consumer>
```
#### Example
```js
// LanguageContext.js

import React, { Component, createContext } from "react";

const LanguageContext = createContext();

class LanguageProvider extends Component {
	constructor(props) {
		super(props);
		this.state = {
			language: "english",
		};

		this.changeLanguage = this.changeLanguage.bind(this);
	}

	changeLanguage(evt) {
		this.setState({ language: evt.target.value });
	}

	render() {
		return (
			<LanguageContext.Provider value={{ ...this.state, changeLanguage: this.changeLanguage }}>
				{this.props.children}
			</LanguageContext.Provider>
		);
	}
}

// HOC that takes a component as an argument and pass down its props.
// Then returns that same component with a prop from LanguageContext.Consumer
// along with all of its original props.
const withLanguageContext = (Component) => (props) => (
	<LanguageContext.Consumer>
		{(value) => <Component languageContext={value} {...props} />}
	</LanguageContext.Consumer>
);

export { LanguageContext, LanguageProvider, withLanguageContext };
```
```js
// Navbar.js

import React, { Component } from "react";
import AppBar from "@mui/material/AppBar";
import Switch from "@mui/material/Switch";
import { ThemeContext } from "./contexts/ThemeContext.js";
import { withLanguageContext } from "./contexts/LanguageContext.js";

const content = {
	english: {
		flag: "🇬🇧",
	},
	french: {
		flag: "🇫🇷",
	},
	spanish: {
		flag: "🇪🇸",
	},
};

class Navbar extends Component {
	// ThemeContext directly consumed.
	static contextType = ThemeContext;
	
	render() {
		const { darkMode, toggleMode } = this.context;
		const { language } = this.props.languageContext;
		
		const { flag } = content[language];
		
		return (
			<AppBar position="static" color={darkMode ? "default" : "primary"}>
				<Switch color="default" onChange={toggleMode} />
				<span>{flag}</span>
			</AppBar>
		);
	}
}

// LanguageContext passed in as a prop through the HOC withLanguageContext.
export default withLanguageContext(Navbar);
```
```js
// Greet.js

import React, { Component } from "react";
import { Select } from "@mui/material";
import { MenuItem } from "@mui/material";
import { LanguageContext } from "./contexts/LanguageContext.js";

const words = {
	english: {
		greeting: "Hello",
	},
	french: {
		greeting: "Bonjour",
	},
	spanish: {
		greeting: "Hola"
	},
}

class Greet extends Component {
	static contextType = LanguageContext;
	
	render() {
		const { language, changeLanguage } = this.context;
		const { greeting } = words[language];
		
		return (
			<main>
				<Select value={language} onChange={changeLanguage}>
					<MenuItem value="english">English</MenuItem>
					<MenuItem value="french">Français</MenuItem>
					<MenuItem value="spanish">Español</MenuItem>
				</Select>
				<h1>{greeting}</h1>
			</main>
		);
	}
}

export default Greet;
```
```js
// App.js

import { ThemeProvider } from "./contexts/ThemeContext.js";
import { LanguageProvider } from "./contexts/LanguageContext.js";
import PageContent from "./PageContent.js";
import Navbar from "./Navbar.js";
import Greet from "./Greet.js";

function App() {
	return (
		<ThemeProvider>
			<LanguageProvider>
				<PageContent>
					<Navbar />
					<Greet />
				</PageContent>
			</LanguageProvider>
		</ThemeProvider>
	);
}

export default App;
```

## Hooks-based Approach
### `useContext()` Hook
- Accepts a Context object (which is returned from React.createContext) and returns the current context value for that Context.
- ***Can be used more than once in a given component.***
	- No need for HOC to use multiple Contexts.
### Syntax
```js
// React looks up to find the nearest provider
const value = useContext(MyContext)
```
### Example 1 - Update Contexts to Function-based Components
```js
// LanguageContext.js

import React, { useState, createContext } from "react";

const LanguageContext = createContext();

function LanguageProvider(props) {
	const [language, setLanguage] = useState("english");

	const changeLanguage = (evt) => {
		setLanguage(evt.target.value);
	};

	return (
		<LanguageContext.Provider value={{ language, changeLanguage }}>
			{props.children}
		</LanguageContext.Provider>
	);
}

export { LanguageContext, LanguageProvider };
```
```js
// ThemeContext.js

import React, { useState, createContext } from "react";

const ThemeContext = createContext();

function ThemeProvider(props) {
	const [darkMode, setDarkMode] = useState(true);
	const toggleMode = () => {
		setDarkMode(!darkMode);
	}
	
	return (
		<ThemeContext.Provider value={{ darkMode, toggleMode }}>
			{this.props.children}
		</ThemeContext.Provider>
	);
}

export { ThemeContext, ThemeProvider };
```
### Example 2 - Consume One Context
```js
// Greet.js

import React, { useContext } from "react";
import { Select } from "@mui/material";
import { MenuItem } from "@mui/material";
import { LanguageContext } from "./contexts/LanguageContext.js";

const words = {
	english: {
		greeting: "Hello",
	},
	french: {
		greeting: "Bonjour",
	},
	spanish: {
		greeting: "Hola"
	},
}

function Greet(props) {
	const { language, changeLanguage } = useContext(LanguageContext);
	const { greeting } = words[language];
	
	return (
		<main>
			<Select value={language} onChange={changeLanguage}>
				<MenuItem value="english">English</MenuItem>
				<MenuItem value="french">Français</MenuItem>
				<MenuItem value="spanish">Español</MenuItem>
			</Select>
			<h1>{greeting}</h1>
		</main>
	);
}

export default Greet;
```
### Example 3 - Consume Multiple Context
```js
// Navbar.js

import React, { Component } from "react";
import AppBar from "@mui/material/AppBar";
import Switch from "@mui/material/Switch";
import { ThemeContext } from "./contexts/ThemeContext.js";
import { LanguageContext } from "./contexts/LanguageContext.js";

const content = {
	english: {
		flag: "🇬🇧",
	},
	french: {
		flag: "🇫🇷",
	},
	spanish: {
		flag: "🇪🇸",
	},
};

function Navbar(props) {
	const { darkMode, toggleMode } = useContext(ThemeContext);
	const { language } = useContext(LanguageContext);
	const { flag } = content[language];
	
	return (
		<AppBar position="static" color={darkMode ? "default" : "primary"}>
			<Switch color="default" onChange={toggleMode} />
			<span>{flag}</span>
		</AppBar>
	);
}

export default Navbar;
```


## Reference
[Context - React](https://reactjs.org/docs/context.html#gatsby-focus-wrapper)  
[`createContext()` and `Context.Provider`](https://reactjs.org/docs/context.html#reactcreatecontext)  
[`useContext`](https://reactjs.org/docs/hooks-reference.html#usecontext)
