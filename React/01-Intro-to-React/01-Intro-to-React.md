# Intro to React

## Table of Contents
- [What is it?](#what-is-it)
- [Components](#components)
  - [Creating Components](#creating-components)
    - [Class-based Components](#class-based-components)
    - [Funciton-based Components](#function-based-components)
- [Standard React App Layout](#standard-react-app-layout)
- [JSX](#jsx)
  - [Basic JSX Rules](#basic-jsx-rules)
  - [Embedding JS in JSX](#embedding-js-in-jsx)
  - [Conditionals in JSX](#conditionals-in-jsx)

## What is it?
- Front-end framework/library for building user interfaces.
- Helps us make individual components and then combine them to make larger applications.
  - Components are JS logic + HTML + CSS that are combined into one modular component/piece of an application.
- Usually combined with other tools like React Router, Webpack, Redux, etc.
### Main Goals
- Make it easy to make reusabale "view components".
- These "encapsulate" logic and HTML into a class.
- Often make it easier to build modular applications.
### Server
- Always need to start up a server because babel doesn't work on file protocol.

## Components
- The building blocks of React.
  - Components let you split the UI into independent, reusable pieces and think about each piece in isolation.
- *Reusable pieces of JS logic, HTML, CSS that are combined into one modular component that can then be used to build a larger application.*
  - Typically combine UI (HTML/CSS) with JS logic, into a single wrapped up component.
- Classes that know how to render themselves into HTML.
### Creating Components
#### Class-based Components
- The "traditional" React component.
- Write logic in a JS Class.
- Must include a render method that returns something.
  - `render() { return (<jsx>)}`
  - **Can return only one thing (ex: container/wrapper `div`), which can consist of multiple things.**
##### Example
```html
<!-- index.html -->

<body>
  <div id="root"><!-- component will go in this div --></div>

  <!-- React CDN Links -->
  <script
      crossorigin
      src="https://unpkg.com/react@17/umd/react.development.js"
  ></script>
  <script
      crossorigin
      src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"
  ></script>

  <!-- Transcompiler for JSX -->
  <script src="https://unpkg.com/babel-standalone"></script>

  <!-- type="text/jsx" to indicate that there might be jsx syntax -->
  <script src="index.js" type="text/jsx"></script>
</body>
```
```js
// index.js

// Class-based Component
class Hello extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, there!</h1>
        <h1>Hello, there!</h1>
        <h1>Hello, there!</h1>
      </div>
    );
  }
}

// Indicate what to render and to where.
ReactDOM.render(<Hello />, document.getElementById("root"));
```
#### Function-based Components
- Historically used for simpler "dumb" components.
- Write logic in a JS function.
- No render method needed, just return content.
##### Example
```js
// index.js

// Function-based Component
function Hello() {
    return (
        <div>
            <h1>Hello, there!</h1>
            <h1>Hello, there!</h1>
            <h1>Hello, there!</h1>
        </div>
    );
}

ReactDOM.render(<Hello />, document.getElementById("root"));
```
#### Class vs. Function Components
- Both can accept props and render content.
- Historically, function components couldn't use important features like:
  - State
  - Lifecycle methods
- However, with the introduction of Hooks, we can now write full-featured function components.
### Reference
[Components and Props - React](https://reactjs.org/docs/components-and-props.html)

## Standard React App Layout
- **1 component per file, with capitalized component name as filename.**
  - Keep individual components in separate js files.
  - Ex: `Hello` component in Hello.js, and not in index.js
- **Create an App component file App.js**
  - Top-level component is called *App*.
  - Combines whatever components that we want into a single element/component that we then render.
  - It's usually the only thing that's being rendered in index.js.
  - Example
    ```js
    class App extends React.Components {
      render() {
        return (
          <div>
            <h1>Greetings!</h1>
            <Hello />
            <Goodbye />
          </div>
        )
      }
    }
    ```
- ***Order of script tags in index.html.***
  - Prerequisite components need to be run first.
### Example
![Layout Example index.html](refImg/layout-example-index-html.png)
![Layout Example index.js](refImg/layout-example-index-js.png)
![Layout Example Hello.js](refImg/layout-example-hello-js.png)
![Layout Example Numpicker.js](refImg/layout-example-numpicker-js.png)

## JSX
- JavaScript Syntax Extension / JavaScript + XML
- Tool that allows us to write HTML-looking code directly in JS.
- Allows us to combine our UI with our JS logic directly in the script rather than having a separate template file in HTML that we then call up in JS.
- JSX is not legal JavaScript.
  - Therefore, it has to be "transpiled" to JavaScript using a transpiler like Babel.
  - ***Babel converts the HTML characters into valid JS code.***
### Basic JSX Rules
  - Elements must either:
    - have an explicit closing tag: `<b>...</b>`
    - be explicitly self-closed: `<input name="msg"/>`
      - Leaving out `/` will result to syntax error.
### Embedding JS in JSX
- `{}` escapes JSX and allows JS expressions.
#### Example
```js
// Embedding JS in JSX

function getMood() {
  const moods = ["Angry", "Hungry", "Silly", "Quiet", "Paranoid"];
  return moods[Math.floor(Math.random() * moods.length)];
}

class JSXDemo extends React.Component {
  render() {
    return (
      <div>
        <h1>My current modd is: {getMood()}</h1>
      </div>
    );
  }
}

ReactDOM.render(<JSXDemo />, document.getElementById("root"));
```
### Conditionals in JSX
#### Example 1
```js
// Conditionals Pattern 1

function getNum() {
  return Math.floor(Math.random() * 10) + 1;
}

class NumPicker extends React.Component {
  render() {
    const num = getNum();
    
    return (
      <div>
        <h1>Your number is: {num}</h1>
        <p>{num === 7 ? "Congrats!" : "Unlucky!"}</p>
        {num === 7 ? (
          <img src="https://i.giphy.com/media/nXxOjZrbnbRxS/giphy.webp" />
        ) : null}
      </div>
    );
  }
}

ReactDOM.render(<NumPicker />, document.getElementById("root"));
```
#### Example 2
```js
// Conditionals Pattern 2

function getNum() {
    return Math.floor(Math.random() * 10) + 1;
}

class NumPicker extends React.Component {
  render() {
    const num = getNum();
    let msg;
    if (num === 7) {
      msg = (
        <div>
          <h2>Congrats! You Win!</h2>
          <img src="https://i.giphy.com/media/nXxOjZrbnbRxS/giphy.webp" />
        </div>
      );
    } else {
      msg = <h2>Sorry! You Lose!</h2>;
    }
    
    return (
      <div>
        <h1>Your number is: {num}</h1>
        {msg}
      </div>
    );
  }
}

ReactDOM.render(<NumPicker />, document.getElementById("root"));
```
### Reference
[Introducing JSX - React](https://reactjs.org/docs/introducing-jsx.html)  
[JSX In Depth - React](https://reactjs.org/docs/jsx-in-depth.html)  
[ConditionalRendering - React](https://reactjs.org/docs/conditional-rendering.html)




