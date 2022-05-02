# Data Binding

## Table of Contents
- [What is Data Binding?](#what-is-data-binding)
- [One-Way Data Binding](#one-way-data-binding)
- [Two-Way Data Binding](#two-way-data-binding)
- [Data Binding in Libraries/Frameworks](#data-binding-in-librariesframeworks)

## What is Data Binding?
> ... a general technique that binds data sources from the provider and consumer together and synchronizes them. | Wikipedia

> In a data binding process, each data change is reflected automatically by the elements that are bound to the data. The term data binding is also used in cases where an outer representation of data in an element changes, and the underlying data is automatically updated to reflect this change. As an example, a change in a TextBox element could modify the underlying data value. | Wikipedia

- Data binding is about connecting values that are in different sources.
  - i.e., connecting and synchronizing data in the View and the Model (usually a database).
- **Property Binding:** Source-to-View.
- **Event Binding:** View-to-Source.
### Effects of Data Binding
- Minimize code needed to connect data with UI.
  - No need to find elements and manually get their values.
- View is automatically updated when the Model(data) changes.
- Can provide real-time updates based on user interaction.

## One-Way Data Binding
- Changes made to the Model will be applied to the View.
- However, the Model is unaware of any changes made in the View.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/one-way-data-binding.png" alt="One-Way Data Binding Illustration" width="80%"/>
</p>

### Pros
- Renders the DOM without hindrance to performance.
- Since data flows one-way, it is easy to understand the code and debug.
### Cons
- You have to write code that detects change and update the View.
  - React performs one-way data binding where the changes are passed on from parent View to child View.

## Two-Way Data Binding
- Changes made on both the ends will be applied to the other end.
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/two-way-data-binding.png" alt="Two-Way Data Binding Illustration" width="80%" />
</p>

### Pros
- Reduces the amount of code needed.
### Cons
- The whole DOM has to be rerendered whenever a change occurs to the Model. Therefore, it can lead to performance hindrance.
### Example
#### Vanilla JS Example
- In the View, there is a form that edits a user's profile.
- Typing in the inputs (changes made in View) will result to updating the State, which updates the View.
```html
<!-- HTML -->

<body>
  <div>
    <label for="firstName">First Name:</label>
    <input id="firstName" data-model="firstName" type="text" />
    <span data-binding="firstName"></span>
  </div>
  <div>
    <label for="lastName">Last Name:</label>
    <input id="lastName" data-model="lastName" type="text" />
    <span data-binding="lastName"></span>
  </div>
  <div>
    <label for="age">Age:</label>
    <input id="age" data-model="age" type="text" />
    <span data-binding="age"></span>
  </div>
  <div>
    <label for="bio">bio</label>
    <input id="bio" data-model="bio" type="text" />
    <span data-binding="bio"></span>
  </div>
  <script src="app.js"></script>
</body>
```
```js
// app.js

const state = {
  firstName: "",
  lastName: "",
  age: null,
  bio: ""
};

// Event Binding - bind View changes to State.
const inputs = document.querySelectorAll("[data-model]"); // inputs.
inputs.forEach((input) => {
  const dataName = input.dataset.model;
  input.addEventListener("keyup", (evt) => {
    state[dataName] = input.value; // Apply View changes to the State.
    render();
  });
});

// Property Binding - bind State changes to View.
const render = () => {
  const viewNames = Array.from(document.querySelectorAll("[data-binding]")).map((el) => el.dataset.binding);
  viewNames.forEach((viewName) => {
    document.querySelector(`[data-binding=${viewName}]`).textContent = state[viewName]; // Apply State changes to the View.
    document.querySelector(`[data-model=${viewName}]`).value = state[viewName]; // Apply State changes to the View.
  });
}

render();
```

## Data Binding in Libraries/Frameworks
### Angular and Vue
- Angular by default implements two-way data binding.
- Vue provides easy to implement two-way data binding through the V-model directive.
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/angular-data-binding.png" alt="Angular Data Binding" width="40%" />
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/vue-data-binding.png" alt="Vue Data Binding" width="40%" />
</p>

### React
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/react-data-binding.png" alt="React Data Binding" width="40%" />
</p>

- **React by default implements only Component to View data binding (i.e., one-way data binding).**
  - If the state changes, the UI will change. But not the other way around.
  - Therefore, one-way data binding is implemented.
- **However, View to Component data binding can also be achieved (effectively, two-way data binding).**
  - Use a controlled input where the element's value is bound to a state variable and an event listener is added to listen to changes in the input and apply those changes to the state.
#### One-Way Data Binding in React
```js
import React, { Component } from 'react';
  
class App extends Component {
  constructor() {
    super();
    this.state = {
      greeting: "Hello"
    };
  }
  
  render() {
    return (
      <div>
        <p>{this.state.greeting}</p>
      </div>
    )
  }
}
  
export default App;
```
#### Two-Way Data Binding in React
```js
const { useState } = React;

const App = () => {
  const [greeting, setGreeting] = useState("Hello");
  const handleChange = (evt) => setGreeting(evt.target.value);
  
  return (
    <div>
      <input type="text" value={greeting} onChange={handleChange} />
      <p>{greeting}</p>
    </div>
  );
};

ReactDOM.render(<App />, document.getElementById('app'));
```


## Reference
[Data binding - Wikipedia](https://en.wikipedia.org/wiki/Data_binding)  
[What is data binding? - YouTube](https://www.youtube.com/watch?v=9G9eYCSdJvU&ab_channel=vaadinofficial)  
[ReactJS Data Binding - GeeksforGeeks](https://www.geeksforgeeks.org/reactjs-data-binding/)  
