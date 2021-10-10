# React Router Patterns

## Table of Contents
- [Route Render Methods and URL Parameters](#route-render-methods-and-url-parameters)
- [Multiple Route Params](#multiple-route-params)
- [404 Route](#404-route)
- [Redirecting](#redirecting)
  - [Client-side Redirects](#client-side-redirects)
  - [Two Options to Redirect the Route](#two-options-to-redirect-the-route)
    - [The `<Redirect>` Component](#the-redirect-component)
    - [Pushing onto the History Prop](#pushing-onto-the-history-prop)
    - [`<Redirect />` vs. Pushing onto `history`](#redirect-vs-pushing-onto-history)
- [`withRouter()` Higher Order Component](#withrouter-higher-order-component)
- [Example](#example)
- [Reference](#reference)

## Route Render Methods and URL Parameters
- The Route component is reponsible for rendering some UI when its path matches the current URL.
- 3 Route Render Methods
  - **`<Route component>`**
    - A React component to render only when the location matches.
    - It uses `React.createElement` to create a new React element from the given component, meaning that providing an inline function to the component prop will create a new component on every render.
      - Hence, use the `render` or the `children` prop when using an inline function for inline rendering.
  - **`<Route render>`**
    - Allows inline rendering and wrapping without the remounting that `component` results in.
    - You can pass in a function to be called when the location matches.
  - **`<Route children>`**
    - Works exactly like `render` but it gets called whether there is a match or not.
    - However, when a route fails to match the URL, then the `match` prop is null.
      - This allows you to dynamically adjust your UI based on whether or not the route matches.
- All three render methods are passed the same three **route props**:
  - **`match`**
    - Contains information about the path and the route that was actually matched.
    - `this.props.match.params.pathName`
  - **`location`**
  - **`history`**
    - A linked list where upon pushing onto it, React Router redirects us to the next route.
  - *Note: these three route props will be passed to each of them.*
    - Ex: `<Route exact path="/food/:name" render={routeProps => <Food {...routeProps} />} />`
      - Pass them as three separate props.

## Multiple Route Params
- Ex: `<Route exact path="food/:foodName/drink/:drinkName" render={routeProps => <Food {...routeProps} />} />`
- Can use `match` to find the params.

## 404 Route
- Add a Route component that does not have path below the other routes.
  - *Note: make sure to wrap the Route components around using the Switch component.*

## Redirecting
### Client-side Redirects
- With React Router, we can mimic the behavior of server-side redirects.
- Useful after certain user actions (ex: submitting a form).
- Also can be used for having a catch-all 404 component.
### Two Options to Redirect the Route
#### The `<Redirect>` Component
- Goes directly in our markup (returned inside the render method).
- Usually used for when user is in the wrong place and should be redirected else where.
##### Example
```js
// Food.js

import React, { Component } from "react";
import { Redirect } from "react-router-dom";

class Food extends Component {
  render() {
    const name = this.props.match.params.name;
    const url = `https://source.unsplash.com/1600x900/?${name}`;
    
    return (
      <div className="Food">
        // Check for numeric digit in `name`.
        {/\d/.test(name) ? (
          <Redirect to="/" />
        ) : (
          <div>
            <h1>I love to eat {name}</h1>
            <img src={url} alt={name} />
          </div>
        )}
      </div>
    );
  }
}

export default Food;
```
#### Pushing onto the History Prop
- Calling `.push` method on `history` route prop.
##### Example
```js
// FoodSearch.js

handleClick() {
  // Do something (ex: save to db)
  alert("saved your food to db!");
  
  // Redirect to somewhere else
  this.props.history.push(`/food/${this.state.query}`);
}
```
#### `<Redirect />` vs. Pushing onto `history`
- The back/forward navigation buttons work differently.
- When using `<Redirect />`, the back button has no evidence that we have visited a certain route (which, we redirected straight away from).
  - Could be preferable to use the `<Redirect />` component in situations where a certain route wasn't supposed to be seen.
- Pushing onto `history`, the back button goes back to the route that we have redirected straight away from (which then bounces back to the redirect).
  - Use for situations in successfully sending someone to a certain route (ex: updated/saved, user did some action).

## `withRouter()` Higher Order Component
- A way connecting components that currently have nothing to do with React Router, with React Router by using `withRouter()`.
- When exporting the component, wrap it around with `withRouter()`.
  - Allows the component to have access to routes, `history`, etc.
- Ex: `export default withRouter(Navbar);`

## Example
```js
// App.js

import React, { Component } from "react";
import { Route } from "react-router-dom";
import Navbar from "./Navbar.js";
import FoodSearch from "./FoodSearch.js";
import Food from "./Food.js";
import Meal from "./Meal.js";
import NotFound from "./NotFound.js";

class App extends Component {
  render() {
    return (
      <div className='App'>
        // Method 1: using component is simpler but doesn't work if you need to pass additional props.
        // <Route exact path="food/:name" component={Food} />

        // Method 2: using render is less clean but more explicit, and you can pass in your own additional props.
        <Navbar />
        <Switch>
          <Route
            exact
            path="/food/:name"
            render={routeProps => <Food {...routeProps} />}
          />
          <Route
            exact
            path="/food/:foodName/drink/:drinkName"
            component={Meal}
          />
          <Route
            exact
            path="/"
            render={(routeProps) => <FoodSearch {...routeProps} />}
          />
          <Route render={() => <NotFound />} />
        </Switch>
      </div>
    );
  }
}

export default App;
```
```js
// Navbar.js

import React, { Component } from "react";
import { withRouter } from "react-router-dom";

class Navbar extends Component {
  constructor(props) {
    super(props);
    this.handleLogin = this.handleLogin.bind(this);
    this.handleBack = this.handleBack.bind(this);
  }
  
  handleLogin() {
    alert("Logged You In!");
    this.props.history.push("/food/salmon");
  }
  
  handleBack() {
    this.props.history.goBack();
  }
  
  render() {
    return (
      <div>
        <button onClick={this.handleBack}>Go Back</button>
        <button onClick={this.handleLogin}>Log In</button>
      </div>
    );
  }
}

export default withRouter(Navbar);
```
```js
// FoodSearch.js

import React, { Component } from "react";

class FoodSearch extends Component {
  constructor(props) {
    super(props);
    this.state = {
      query: "",
    }
    
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }

  handleChange(evt) {
    this.setState({
      query: evt.target.value,
    })
  }
  
  handleClick() {
    // Do something (save to db)
    alert("saved your food to db!");
    // Redirect to somewhere else
    this.props.history.push(`/food/${this.state.query}`);
  }

  render() {
    return (
      <div className="FoodSearch">
        <h1>Search for a Food</h1>
        <input 
          type="text" 
          placeholder="search for a food" 
          value={this.state.query}
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>Save New Food</button>
      <div>
    );
  }
}

export default FoodSearch;
```
```js
// Food.js

import React, { Component } from "react";
import { Redirect } from "react-router-dom";

class Food extends Component {
  render() {
    const name = this.props.match.params.name;
    const url = `https://source.unsplash.com/1600x900/?${name}`;
    
    return (
      <div className="Food">
        {/\d/.test(name) ? (
          <Redirect to="/" />
        ) : (
          <div>
            <h1>I love to eat {name}</h1>
            <img src={url} alt={name} />
          </div>
        )}
      </div>
    );
  }
}

export default Food;
```
```js
// Meal.js

import React, { Component } from "react";

class Meal extends Component {
  render() {
    const foodName = this.props.match.params.foodName;
    const drinkName = this.props.match.params.drinkName;
    const foodUrl = `https://source.unsplash.com/1600x900/?${foodName}`;
    const drinkUrl = `https://source.unsplash.com/1600x900/?${drinkName}`;

    return (
      <div>
        <h1>
          THIS IS A MEAL OF {foodName} + {drinkName}
        </h1>
        <img src={foodUrl} />
        <img src={drinkUrl} />
      </div>
    );
  }
}

export default Meal;
```
```js
// NotFound.js
import React, {Component} from "react";

class NotFound extends Component {
  render() {
    return (
      <h1>404: Not Found</h1>
    );
  }
}

export default NotFound;
```

## Reference
[React Router: Declarative Routing for React.js](https://reactrouter.com/web/api/Route/route-props)  
