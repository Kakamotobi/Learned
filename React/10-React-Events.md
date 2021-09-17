# React Events

## Table of Contents
- [Common Events](#common-events)
- [`this` Keyword and `bind`](#this-keyword-and-bind)
- [Method Binding with Arguments](#method-binding-with-arguments)
- [Passing Functions to Child Components](#passing-functions-to-child-components)

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
