# React Router Patterns

## Table of Contents
- [Route Render Methods and URL Parameters](#route-render-methods-and-url-parameters)
- [Multiple Route Params](#multiple-route-params)
- [Example](#example)
- [404 Route](#404-route)
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
  - *Note: these three route props will be passed to each of them.*
    - Ex: `<Route exact path="/food/:name" render={routeProps => <Food {...routeProps} />} />`
      - Pass them as three separate props.

## Multiple Route Params
- Ex: `<Route exact path="food/:foodName/drink/:drinkName" render={routeProps => <Food {...routeProps} />} />`

## Example
```js
// App.js

import React, { Component } from "react";
import { Route } from "react-router-dom";
import Food from "./Food.js";
import Meal from "./Meal.js";

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
        </Switch>
      </div>
    );
  }
}

export default App;
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

## 404 Route
- Add a Route component that does not have path below the other routes.
  - *Note: make sure to wrap the Route components around using the Switch component.*
### Example
```js
// Routes.js

import React, {Component} from "react";
import { Route, Switch } from "react-router-dom";
import NotFound from "./NotFound.js"

class Routes extends Component {
  render() {
    return (
      <Switch>
        <Route exact path="/about" render={() => <About />} />
        <Route exact path="/contact" render={(routeProps) => <Contact {...routeProps} />} />
        <Route exact path="/blog/:slug" render={(routeProps) => <BlogPost {...routeProps} />} />
        <Route exact path="/" render={() => <Home />} />
        <Route render={() => <NotFound />} />
      </Switch>
    );
  }
}

export default Routes;
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
```

## Reference
[React Router: Declarative Routing for React.js](https://reactrouter.com/web/api/Route/route-props)  
