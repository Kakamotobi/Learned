# Data Binding

## Table of Contents
- [What is Data Binding?](#what-is-data-binding)
- [One-Way Data Binding](#one-way-data-binding)
- [Two-Way Data Binding](#two-way-data-binding)

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


## Two-Way Data Binding
- Changes made on both the ends will be applied to the other end.
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/two-way-data-binding.png" alt="Two-Way Data Binding Illustration" width="80%" />
</p>

### Example
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

## Reference
[Data binding - Wikipedia](https://en.wikipedia.org/wiki/Data_binding)  
[What is data binding? - YouTube](https://www.youtube.com/watch?v=9G9eYCSdJvU&ab_channel=vaadinofficial)  
