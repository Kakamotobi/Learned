# React Forms

## Table of Contents
- [Forms in React](#forms-in-react)
  - [Controlled Components](#controlled-components)
  - [Example](#example)
- [Handling Multiple Inputs](#handling-multiple-inputs)

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
### Example
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
        <label for="fullname">Full Name: </label>
        <input 
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

