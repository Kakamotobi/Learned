# Intro to React Router

## Table of Contents
- [What is Routing?](#what-is-routing)
- [React Router](#react-router)
- [Reference](#reference)

## What is Routing?
> For SPA in application layer, router is a library that decides what web page is presented by a given URL. This middleware module is used for all URL functions, as these are given a path to a file that is rendered to open the next page. | MDN
- Before, "routing" usually refered to server-side routing (the traditional version of routing).
### Server-Side Routing
- The server decides what HTML to return based on the URL requested, and the entire page refreshes.
  - Ex: clicking an anchor tag causes the browser to request a new page and replace the entire DOM.
### Client-Side Routing
- Client-side routing handles mapping between URL bar and the content a user sees via the browser rather than via the server.
- It is possible to display different information/content to the user based off of the URL without having to refresh the page.
- Sites that exclusively use client-side routing are SPAs.
- The JS is used to manipulate the URL with the "History" Web API.

## React Router
- React Router is not built-in to React as it is its own tool.
### `react-router-dom`
- `npm install react-router-dom`
- Wrap the entire `<App />` component using **`BrowserRouter`** in index.js file.
- **`<Route path="" component={} [exact]/>`**
  - Define routes using the `<Route />` component.
  - The `exact` attribute indicates that the path has to exactly match.
    - Rule of Thumb: include `exact` to all routes unless there is a reason not to.
- **`<Switch></Switch>`**
  - Switch makes sure only one of the routes that match the URL is rendered.
  - Whichever path is matched first will be routed to.
  - If all the routes have the `exact` attribute, we don't have to wrap the routes around with `<Switch>`.
- **`<Link></Link>`**
  - Use `<Link>` components instead of `<a>` tags.
  - Instead of an `href` attribute, `<Link>` uses a `to` property.
  - Clicking on `<Link>` does not issue a GET request (hence, page doesn't refresh).
    - JS intercepts the click and executes client-side routing.
  - *Note: it's not a good idea to put interactive content inside `<Link>`/`<a>` tags. Use history instead.*
- **`<NavLink></NavLink>`**
  - Works like the `<Link>` component with just one additional feature.
    - We can style the `<a>` tags selectively based off of which ones are *active*.
  - Lets us stylize links to the page that the user is "already at" using the `activeStyle` (in-line) or **`activeClassName`** props.
  - Include the `exact` attribute to indicate that the `activeClassName` should only be applied when the path is matched to the current route.
### Render Prop vs. Component Prop in Routes
- Both have a function that returns JSX.
- Both work for what we wish to accomplish.
- Use either one accordingly.
#### Component Prop
- Doesn't reuse an old component. Instead creates a new one each time (going through the whole lifecycle of mounting and unmounting).
- Ex: `<Route exact path="/dog/c" component={() => <Dog name="Muffins" />} />`
- ***Use when not passing in props.***
  - Ex: `<Route exact path="/dog" component={Dog} />`
#### Render Prop
- Doesn't make a new component each time. Instead, reuses/remounts the existing component that has already been made.
- *Use when passing in props.*
- Ex: `<Route exact path="/dog/r" render={() => <Dog name="Muffins" />} />`
### Example
```zsh
// Terminal

npm install react-router-dom
```
```js
// index.js

import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";
import App from "./App.js";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
)
```
```js
// App.js

import React, { Component } from "react";
import { Route, Switch, NavLink } from "react-router-dom"
import About from "./About.js";
import Dog from "./Dog.js";
import Contact from "./Contact.js";
import "./App.css"

class App extends Component {
  render() {
    return (
      <div className="App">
        <nav className="App-nav">
          <NavLink exact to="/" activeClassName="active-link">About</NavLink>
          <NavLink exact to="/contact" activeClassName="active-link">Contact</NavLink>
          <NavLink exact to="/dog" activeClassName="active-link">Dog</NavLink>
          <NavLink exact to="/dog/c" activeClassName="active-link">Dog(c)</NavLink>
          <NavLink exact to="/dog/r" activeClassName="active-link">Dog(r)</NavLink>
        </nav>
        <Switch>
          <Route exact path="/" component={About} />
          <Route exact path="/contact" component={Contact} />
          <Route exact path="/dog/c" component={() => <Dog name="Muffins" />} />
          <Route exact path="/dog/r" render={() => <Dog name="Biscuits" />} />
          <Route exact path="/dog/puppy" component={Puppy} />
        <Switch>
      </div>
    );
  }
}

export default App;
```
```js
// Dog.js

import React, { Component } from "react";

class Dog extends Component {
  render() {
    return (
      <div className="Dog">
        <h1>Dog!</h1>
        <h3>This dog's name is: {this.props.name}</h3>
      </div>
    );
  }
}
```
```css
/* App.css */

.active-link {
  color: blue;
  border-bottom: 1px solid blue;
}
```

## Reference
[Routers - MDN](https://developer.mozilla.org/en-US/docs/Glossary/routers)  
[React Router: Declarative Routing for React.js](https://reactrouter.com/)
