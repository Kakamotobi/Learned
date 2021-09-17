# React Events

## Table of Contents
- [Common Events](#common-events)
- [`this` Keyword and `bind`](#this-keyword-and-bind)
- [Method Binding with Arguments](#method-binding-with-arguments)
- [Passing Functions to Child Components](#passing-functions-to-child-components)
- [React Lists and Keys](#react-lists-and-keys)

## Common Events
- **`onMouseEnter`**
- **`onMouseLeave`**
- **`onKeyUp`**
- **`onKeyDown`**
- **`onKeyPress`**
- **`onCopy`**

## `this` Keyword and `bind`
- When your event handlers reference the keyword `this`, you will lose the `this` context when you pass a function as a handler.
- So make sure to bind your event handlers to the individual component.
### 3 Ways to Bind `this`
- **Use `bind` inline**
  - Pros
    - Very explicit
  - Cons
    - When you need to pass the function to multiple components, you're going to be making a copy for each component.
    - Every time `bind` is called, a new function is created, so binding inside render will create a lot of functions.
  - Example
    ```js
    <div className="WiseSquare" onMouseEnter={this.dispenseWisdom.bind(this)}>
      {/" "/}
    </div>
    ```
- **Use an arrow function rather than manually binding**
  - Pros
    - No need to use `bind`.
  - Cons
    - Intent is less clear.
    - When you need to pass the function to multiple components, you're going to be making a copy for each component.
    - New function is created on every render.
  - Example
    ```js
    <div className="WiseSquare" onMouseEnter={() => this.dispenseWisdom()}>
      {/" "/}
    </div>
    ```
- **Method bind in the constructor**
  - Pros
    - Bind once in the constructor and be done with it.
    - More performant
  - Cons
    - Clunkier
  - Example
    ```js
    class WiseSquare extends Component {
      constructor(props) {
        super(props);
        this.dispenseWisdom = this.dispenseWisdom.bind(this);
      }
    }

    <div className="WiseSquare" onMouseEnter={this.disepenseWisdom}>
      {/" "/}
    </div>
    ```
### Alternative Way to Bind
- The experimental public class fields syntax.
- Need to rely on babel to make it work.
- Example
  ```js
  class WiseSquare extends Component {
    disepenseWisdom = () => {
      // code to dispense wisdom
    }
    
    render() {
      return (
        <div className="WiseSquare" onMouseEnter={this.disepenseWisdom}>
          {/" "/}
        </div>
      );
    }
  }
  ```

## Method Binding with Arguments
### Example
```js
class ButtonList extends Component {
  static defaultProps = {
    colors: ["#e056fd", "#eb4d4b", "#badc58", "#f0932b"]
  };
  constructor(props) {
    super(props);
    this.state = { color: "cyan" };
  }

  changeColor(newColor) {
    console.log(`newColor is: ${newColor}`);
    this.setState({ color: newColor });
  }

  render() {
    return (
      <div className='ButtonList' style={{ backgroundColor: this.state.color }}>
        {this.props.colors.map(c => {
          const colorObj = { backgroundColor: c };
          return (
            <button style={colorObj} onClick={this.changeColor.bind(this, c)}>
            // OR
            // <button style={colorObj} onclick={() => this.changeColor(c)}>
              Click on me!
            </button>
          );
        })}
      </div>
    );
  }
}

export default ButtonList;
```

## Passing Functions to Child Components
- The idea: child components are often not stateful, but need to tell parents to change state.
- How we send data "back up" to a parent component.
### Flow
- A parent component defines a function.
- The function is passed as a prop to a child component.
- The child component invokes the function as a prop.
- That function is called in the parent, usually setting new state.
- The parent component is re-rendered along with its children.
### Rules to Binding
- Don't bind in the child component if not needed - bind in parent.
- If you need a parameter, pass it down to the child as a prop, then bind in parent and child.
- Avoid inline arrow functions/binding in render if possible.
### Example 1 (Not so good)
```js
// NumberList.js

class NumberList extends Component {
  constructor(props) {
    super(props);
    this.state = { nums: [1, 2, 3, 4, 5] };
  }

  remove(num) {
    this.setState(st => ({
      nums: st.nums.filter(n => n !== num)
    }));
  }

  render() {
    let nums = this.state.nums.map(n => (
      <NumberItem value={n} remove={() => this.remove(n)} />
    ));
    return (
      <div>
        <h1>First Number List</h1>
        <ul>{nums}</ul>
      </div>
    );
  }
}

export default NumberList;
```
```js
// NumberItem.js

class NumberItem extends Component {
  render() {
    return (
      <li>
        {this.props.value}
        <button onClick={this.props.remove}>X</button>
      </li>
    );
  }
}

export default NumberItem;
```
### Example 2 (Better Way)
- Pass the function as is to the child component.
- On the child component's part, create a handler and bind the handler to the instance.
```js
// NumberList.js

class BetterNumberList extends Component {
  constructor(props) {
    super(props);
    this.state = { nums: [1, 2, 3, 4, 5] };
    this.remove = this.remove.bind(this);
  }

  remove(num) {
    console.log("REMOVING!");
    console.log(num);
    this.setState(st => ({
      nums: st.nums.filter(n => n !== num)
    }));
  }

  render() {
    // Key must be unique!
    let nums = this.state.nums.map(n => (
      <BetterNumberItem key={n} value={n} remove={this.remove} />
    ));
    return (
      <div>
        <h1>Better Number List</h1>
        <ul>{nums}</ul>
      </div>
    );
  }
}

export default BetterNumberList;
```
```js
class BetterNumberItem extends Component {
  constructor(props) {
    super(props);
    this.handleRemove = this.handleRemove.bind(this);
  }
  handleRemove(evt) {
    console.log("INSIDE HANDLE REMOVE");
    this.props.remove(this.props.value);
  }
  render() {
    return (
      <li>
        {this.props.value}
        <button onClick={this.handleRemove}>X</button>
      </li>
    );
  }
}

export default BetterNumberItem;
```
### Parent-Child Method Naming Conventions
- For consistency, try to follow the `action` / `handleAction` pattern.
  - Ex: `remove` in the parent / `handleRemove` in the child.

## React Lists and Keys
- "Warning: Each child in an array or iterator should have a unique "key" prop."
- This warning appears when mapping over data and returning components.
- **Key** is a special string attribute to include when creating lists of elements.
> Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity.
### Use Libraries
- Ex: shortid, uuid
