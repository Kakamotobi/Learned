# Component Lifecycle Methods

## Table of Contents
- [What are Component Lifecycle Methods?](#what-are-component-lifecycle-methods)
  - [3 Main Phases](#3-main-phases)
    - [1) Mounting](#1-mounting)
    - [2) Updating](#2-updating)
    - [3) Unmounting](#3-unmounting)

## What are Component Lifecycle Methods?
- Every component comes with methods that allow developers to update application state and reflect the changes to the UI before/after key react "events" (events/stages in the lifecycle of a component).
### 3 Main Phases
- For every component, when we render it (ex: to App.js), there are 3 stages.
- When rendering a component, the very first thing it does is to run the constructor.
  - The constructor is used to initialize state and to bind event handlers.
- After the constructor, React calls `render()`. React updates the DOM to match the output of `render()`.
- After `render()` has been called, we have access to `componentDidMount`.
#### 1) Mounting
- **`componentDidMount()`** is a lifecycle method that we can define, which React will automatically run it for us **once** after the component has mounted (after the first render to the DOM) and will not run for subsequent re-renders.
- **"Mounting"** refers to the first time the component is rendered to the DOM.
- It is a good place to load any data via AJAX or set up subscriptions/timers.
- *Note: calling `setState()` here will trigger re-render.*
  - By the time we get to `componentDidMount()`, render has already been called once. So setting the state in `componentDidMount()` will re-render.
##### Example 1
- Starting a timer when Clock instance is first rendered to the DOM.
```js
class Timer extends Component {
  constructor(props) {
    super(props);
    this.state = {
      time: new Date(),
    }
    console.log("In Constructor");
  }

  componentDidMount() {
    console.log("In ComponentDidMount");
    this.timerID = setInterval(() => {
      this.setState({ time: new Date() })
    }, 1000);
  }

  render() {
    console.log("In Render");
    return <h1>{this.state.time.getSeconds()}</h1>
  }
}
```
```js
// In Constructor
// In Render
// In ComponentDidMount
// In Render <-- on repeat
```
##### Example 2 - AJAX and loader
- **Anytime we're fetching data, it is conventional to fetch it in `componentDidMount()`, and not the constructor.**
```js
// ZenQuote.js

import React, { Component } from "react";
import axios from "axios";
import "./ZenQuote.css"

class ZenQuote extends Component {
  constructor(props) {
    super(props);
    this.state = { 
      quote: "",
      isLoaded: false,
    }
  }
  
  componentDidMount() {
    // Load Data
    axios.get("https://api.github.com/zen")
      .then(res => {
        // Set State with that Data
        this.setState({ 
          quote: res.data,
          isLoaded: true
        })
      });
  }

  render() {
    return (
      <div>
        {this.state.isLoaded ? (
         <div>
            <h1>Always remember...</h1>
            <p>{this.state.quote}</p>
         </div>
        ) : (
          <div className="loader"></div>
        )}
      </div>
    );
  }
}

export default ZenQuote;
```
##### Example 3 - async functions
```js
// App.js
import React, { Component } from "react:
import GithubUserInfo from "./GithubUserInfo.js"

class App extends Component {
  render() {
    return (
      <div classname="App">
        <GithubUserInfo username="facebook" />
        <GithubUserInfo username="Kakamotobi" />
      </div>
    );
  }
}
```
```js
//GithubUserInfo.js
import React, { Component } from "react:
import axios from "axios";

class GithubUserInfo extends Component {
  constructor(props) {
    super(props);
    this.state = {
      user: {}
    }
  }
  
  async componentDidMount() {
    const url = `https://api.github.com/users/${this.props.username}`
    const res = await axios.get(url);
    const data = res.data;
    this.setState({
      user: data
    });
  }
  
  render() {
    const {name, bio, avatar_url} = this.state.user;
    
    return (
      <div>
        <h1>{name}</h1>
        <p>{bio}</p>
        <img src={avatar_url} />
      </div>
    )
  }
}

export default GithubUserInfo;
```

#### 2) Updating

#### 3) Unmounting
