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
- 

## React Router
- React Router is not built-in to React as it is its own tool.
### `react-router-dom`
- `npm install react-router-dom`
- Wrap the entire `<App />` component in **`BrowserRouter`**.
- **`<Route path="" component={} [exact]/>`**
  - Define routes using the `<Route />` component.
  - The `exact` attribute indicates that the path has to exactly match.
    - Rule of Thumb: include `exact` to all routes unless there is a reason not to.
- **`<Switch></Switch>`**
  - Switch makes sure only one of the routes that match the URL is rendered.
  - Whichever path is matched first will be routed to.
  - If all the routes have the `exact` attribute, we don't have to wrap the routes around with `<Switch>`.
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
import { Route, Switch } from "react-router-dom"
import About from "./About.js";
import Dog from "./Dog.js";
import Contact from "./Contact.js";

class App extends Component {
  render() {
    return (
      <div className="App">
        <Switch>
          <Route exact path="/" component={About} />
          <Route exact path="/contact" component={Contact} />
          <Route exact path="/dog" component={Dog} />
          <Route exact path="/dog/puppy" component={Puppy} />
        <Switch>
      </div>
    );
  }
}

export default App;
```

## Reference
[Routers - MDN](https://developer.mozilla.org/en-US/docs/Glossary/routers)  
[React Router: Declarative Routing for React.js](https://reactrouter.com/)
