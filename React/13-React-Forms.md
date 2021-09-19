# React Forms

## Table of Contents
- [Forms in React](#forms-in-react)
  - [Controlled Components](#controlled-components)
  - [Simple Example](#simple-example)
- [Handling Multiple Inputs](#handling-multiple-inputs)
  - [Computed Property Names](#computed-property-names)
  - [Multiple Inputs Example](#multiple-inputs-example)
- [Passing Data Upwards](#passing-data-upwards)
  - [Upward Data Flow Example](#upward-data-flow-example)

## Forms in React
- There will be a function that handles the submission of the form AND has access to the data that he user entered.
  - The technique to get this is ***controlled components***.
### Controlled Components
- In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input.
- In React, mutable state is kept in the *state* of components, and only updated with `setState()`.
- So, how do we have React state be 100% in sync with the `<input>` in the form (instead of having to wait for submit)?
  - Make React control:
    - What is shown (the value of the component)
    - What happens when the user types (this gets kept in state)
  - Input elements controlled in this way are called ***controlled components***.
### Simple Example
```js
class NameForm extends Component {
  constructor(props) {
    super(props);
    this.state = { fullName: "" );
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  
  handleSubmit(evt) {
    evt.preventDefault();
    // do something with form data.
  }
  
  handleChange(evt) {
    // runs on every keystroke event.
    // update the corresponding state everytime the input value changes.
    this.setState({ fullName: evt.target.value });
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label htmlFor="fullname">Full Name: </label>
        <input
          id="fullname"
          name="fullname" 
          value={this.state.fullName} // Set the value to the corresponding state.
          onChange={this.handleChange} // Everytime this input changes, handleChange runs.
        />
        <button>Add</button>
      </form>
    );
  }
}
```
- Since the value attribute is set on the input element, the displayed value will always be `this.state.fullName` - making the React state the source of truth.
- Since `handleChange` runs on every keystroke to update the React state, the displayed value will update as the user types.
- With a controlled component, every state mutation will have an associated handler function. This makes it easy to modify or validate user input.

## Handling Multiple Inputs
### Computed Property Names
- Example
  ```js
    let microchip = 143214241;
    let catData = {
      // property computed inside the object literal
      [microchip]: "Blue Steele"
    };
  ```
- **Using this, instead of making a separate `onChange` handler for every single input, we can make a generic function for multiple inputs.**
- *To handle multiple controlled inputs, add the HTML **name** attribute to each JSX input element (exactly matching the name used in the state) and let the handler function decide the appropriate key in state to update based on `evt.target.name`.*
  - Example
    ```js
    class Mycomponent extends Component {
      // ...
      handleChange(evt) {
        this.setState({
          [evt.target.name]: evt.target.value
        });
      }
      // ...
    }
    ```
### Multiple Inputs Example
```js
class Form extends Component {
  constructor(props) {
    super(props);
    this.state = { fullName: "", email: "", password: "" );
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  
  handleSubmit(evt) {
    evt.preventDefault();
    // do something with form data.
  }
  
  handleChange(evt) {
    // runs on every keystroke event.
    // update the corresponding state everytime the input value changes.
    this.setState({
      [evt.target.name]: evt.target.value
    });
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label htmlFor="fullname">Full Name: </label>
        <input 
          id="fullname"
          name="fullname"
          value={this.state.fullName} // Set the value to the corresponding state.
          onChange={this.handleChange} // Everytime this input changes, handleChange runs.
        />
        <label htmlFor="email">Email: </label>
        <input 
          id="email"
          name="email" 
          value={this.state.email}
          onChange={this.handleChange}
        />
        <label htmlFor="password">Password: </label>
        <input
          id="password"
          name="password"
          value={this.state.password}
          onChange={this.handleChange}
        />
        <button>Add</button>
      </form>
    );
  }
}
```

## Passing Data Upwards
- *Define the **method** to be passed on to the child component and the **target state** in the parent component.*
- Call that method and update the state from the child component.
### Upward Data Flow Example
```js
// ShoppingList.js

class ShoppingList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      items: [
        { name: "Milk", qty: "2 gallons", id: uuid() },
        { name: "Bread", qty: "2 loaves", id: uuid() }
      ]
    };
    this.addItem = this.addItem.bind(this);
  }
  
  // Define method to be passed on to the child component in the parent component.
  addItem(item) {
    let newItem = { ...item, id: uuid() };
    this.setState(state => ({
      items: [...state.items, newItem]
    }));
  }
  
  renderItems() {
    return (
      <ul>
        {this.state.items.map(item => (
          <li key={item.id}>
            {item.name}:{item.qty}
          </li>
        ))}
      </ul>
    );
  }
  
  render() {
    return (
      <div>
        <h1>Shopping List</h1>
        {this.renderItems()}
        <ShoppingListForm addItem={this.addItem} />
      </div>
    );
  }
}

export default ShoppingList;
```
```js
// ShoppingListForm.js

class ShoppingListForm extends Component {
  constructor(props) {
    super(props);
    this.state = { name: "", qty: "" };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  
  handleSubmit(evt) {
    evt.preventDefault();
    this.props.addItem(this.state);
    this.setState({ name: "", qty: "" });
  }
  
  handleChange(evt) {
    this.setState({
      [evt.target.name]: evt.target.value
    });
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label htmlFor='name'>Name: </label>
        <input
          id='name'
          name='name'
          value={this.state.name}
          onChange={this.handleChange}
        />
        <label htmlFor='qty'>Quantity: </label>
        <input
          id='qty'
          name='qty'
          value={this.state.qty}
          onChange={this.handleChange}
        />
        <button>Add Item</button>
      </form>
    );
  }
}

export default ShoppingListForm;
```
