# Properties and More

## Table of Contents
- [Properties](#properties)
  - [Using Properties](#using-properties)
  - [Properties Requirements](#properties-requirements)
  - [Other Types of Properties](#other-types-of-properties)
  - [Default Props](#default-props)
  - [`props.children`](#propschildren)
- [Looping in JSX](#looping-in-jsx)
- [The 2 Ways of Styling React](#the-2-ways-of-styling-react)

## Properties (Props)
- Way of passing data from one component to another (from a parent to a child).
- *A useful component is a reusable one, which often means making it configurable or customizable.*
### Using Properties
- Go to wherever you're calling upon a component and pass data inside the component tag (just like passing in a `src` attribute for an image).
- `console.log(this.props)` to see props object.
#### Example
```js
// index.js

class App extends React.Component {
  render() {
    return (
      <div>
        <Hello to="Tom" from="Jerry" />
        <Hello to="Kakamotobi" from="Bomboclat" />
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById("root")
```
```js
// Hello.js

class Hello extends React.component {
  render() {
    console.log(this.props);
    
    return <p>Hi {this.props.to} from {this.props.from}</p>
  }
}
```
### Properties Requirements
- **Properties are for *configuring* your component.**
- **Properties are *immutable*.**
  - Ex: `this.props.from = "Topsy"` results in an error.
  - *Doesn't mean that the data in your application cannot change or that your components can never be altered.*
  - *But we don't do it through props.*
### Other Types of Properties
- Properties can be strings.
  - Ex: `<User name="Tom" title="Cat"/>`
- For other types (numbers, boolean, arrays, objects, etc.), embed JS expression using `{}`.
  - Ex: `<User name="Tom" salary={999999} hobbies={["chasing", "piano", "games"]}/>`
#### Example
```js
// index.js

class App extends React.Component {
  render() {
    return (
      <div>
        <Hello 
          to="Tom" 
          from="Jerry" 
          numOfBangs={5}
          img="https://images.unsplash.com/photo-1536053341826-c373a78370b4?ixlib=rb-         1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3782&q=80"
        />
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById("root")
```
```js
// Hello.js

class Hello extends React.Component {
  render() {
    const { to, from, img } = this.props;
    let numOfBangs = "!".repeat(this.props.numOfBangs);

    return (
      <div>
        <p>Hi {to} from {from}{numOfBangs}</p>
        <img src={img} />
      </div>
    );
  }
}
```
### Default Props
- **Components can specify default values for missing props.**
- Simply, define an object called **`defaultProps`** with key-value pairs using the **`static`** keyword.
#### Example
```js
//index.js

class App extends React.Component {
  render() {
    return (
      <div>
        <Hello to="Ringo" from="Paul" bangs={4} />
        <Hello to="George" />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```
```js
// Hello.js

class Hello extends React.Component {
  static defaultProps = {
    from: "Anonymous",
    bangs: 1,
  };
  
  render() {
    let bangs = "!".repeat(this.props.bangs);
    
    return (
      <div>
        <p>Hi {this.props.to} from {this.props.from}{bangs}</p>
      </div>
    );
  }
}
```
### `props.children`
- React has a composition model that allows us to reuse code between components.
- Allows other components to pass arbitrary children to the component by nesting the JSX. 
  - This takes whatever was nestled between the opening and closing tag of the wrapping component and displays it in the wrapping component.
  - Ex: anything inside the `<Message></Message>` JSX tag gets passed to into the `Message` component as a `children` prop.
- This is useful for but not limited to styling.
#### Example
```js
// Message.js

import React, { Component } from "react";
import "./Message.css";

class Message extends Component {
  render() {
    return <div className="Message">{this.props.children}</div>;
  }
}

export default Message;
```
```css
/* Message.css */

.Message {
  margin: 1rem;
  padding: 2rem;
  background-color: rgba(0, 0, 0, 0.4);
  border-radius: 10px;
  color: white;
  text-align: center;
  text-transform: uppercase;
}
```
```js
// VendingMachine.js

import React, { Component } from "react";
import { Link } from "react-router-dom";
import Message from "./Message.js";
import "./VendingMachine.css";

class VendingMachine extends Component {
  render() {
    return (
      <div className="VendingMachine">
        <Message>
          <h1>Vending Machine</h1>
        </Message>
        <Message>
          <div>
            <Link to="/soda">Soda</Link>
            <Link to="/chips">Chips</Link>
            <Link to="sardines">Sardines</Link>
          </div>
        </Message>
      </div>
    );
  }
}

export default VendingMachine;
```
```js
// Chips.js

import React, { Component } from "react";
import { Link } from "react-router-dom";
import Message from "./Message.js";
import chips from "./images/Chips.png";
import "./Chips.css";

class Chips extends Component {
  constructor(props) {
    super(props);
    this.state = {
      bagOfChips: [],
    };

    this.eatChips = this.eatChips.bind(this);
  }

  eatChips() {
    const x = window.innerWidth * Math.random();
    const y = window.innerHeight * Math.random();

    this.setState((prevState) => ({
      bagOfChips: [...prevState.bagOfChips, { x, y }],
    }));
  }

  render() {
    const bagOfChips = this.state.bagOfChips.map((c, i) => (
      <img
        key={i}
        className="bag-of-chips"
        src={chips}
        style={{ top: `${c.y}px`, left: `${c.x}px` }}
        alt="Bag of Chips"
      />
    ));

    return (
      <div className="Chips">
        <Message>
          <h2>Chips!</h2>
          <div>
            <h3>Bags Eaten: {this.state.bagOfChips.length}</h3>
            <button type="button" onClick={this.eatChips}>
              NOM NOM NOM
            </button>
          </div>
          <Link to="/vendingmachine">Go Back</Link>
        </Message>
        {bagOfChips}
      </div>
    );
  }
}

export default Chips;
```

## Looping in JSX
### **`array.map(fn)`**
- Common to use `map()` to output loops in JSX.
- Ex: `someData.map` to create more complex markup or other child components, etc.
### Example
```js
// index.js

class App extends React.Component {
  render() {
    return )
      <div>
        <Friend
          name="Elton"
          hobbies={["Piano", "Singing", "Dancing"]}
        />
        <Friend
          name="Frida"
          hobbies={["Drawing", "Painting"]}
        />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```
```js
// Friend.js

class Friend extends React.Component {
  render() {
    const { name, hobbies } = this.props;
    const lis = hobbies.map((hobby) => <li>{hobby}</li>);

    return (
      <div>
        <h1>{name}</h1>
        <ul>{lis}</ul>
      </div>
    );
  }
}
```

## The 2 Ways of Styling React
1. Define CSS classes that you toggle on and off.
    - *JSX uses **`className`** instead of `class`.*
2. Can also add inline CSS styles rather than through a CSS class, by passing in a JS object to `style`.
### Example
```html
<!-- index.html -->

<head>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="root"></div>
  
  <!--  React  -->
  <script src="https://unpkg.com/react/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>

  <!--  Babel  -->
  <script src="https://unpkg.com/babel-standalone"></script>
  
  <!--  Components  -->
  <script src="Machine.js" type="text/jsx"></script>
  <script src="index.js" type="text/jsx"></script>
</body>
```
```js
// index.js

class App extends React.Component {
  render() {
    const rand1 = Math.floor(Math.random() * 3);
    const rand2 = Math.floor(Math.random() * 3);
    const rand3 = Math.floor(Math.random() * 3);
    const rand4 = Math.floor(Math.random() * 3);
    const rand5 = Math.floor(Math.random() * 3);
    const rand6 = Math.floor(Math.random() * 3);
    const rand7 = Math.floor(Math.random() * 3);
    const rand8 = Math.floor(Math.random() * 3);
    const rand9 = Math.floor(Math.random() * 3);
    const fruits = ["🍋", "🍑", "🍒"];

    const reload = document.querySelector("#reload");
    reload.addEventListener("click", function () {
        location.reload();
    });

    return (
      <div>
        <h1>Slot Machines!</h1>
        <Machine
          s1={fruits[rand1]}
          s2={fruits[rand2]}
          s3={fruits[rand3]}
        />
        <Machine
          s1={fruits[rand4]}
          s2={fruits[rand5]}
          s3={fruits[rand6]}
        />
        <Machine
          s1={fruits[rand7]}
          s2={fruits[rand8]}
          s3={fruits[rand9]}
        />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("root"));
```
```js
// Machine.js

class Machine extends React.Component {
  render() {
    const { s1, s2, s3 } = this.props;
    let msg;

    const winner = s1 === s2 && s2 === s3;
    winner ? (msg = "Jackpot!") : (msg = "You Lose!");

    return (
      <div className="Machine">
        <p style={fontSize: "50px", backgroundColor: "purple"}>
          {s1} {s2} {s3}
        </p>
        <p className={winner ? "jackpot" : "lose" }>{msg}</p>
      </div>
    );
  }
}
```
```css
/* style.css */

.Machine {
    border: 2px solid black;
    background: lightblue;
    width: 200px;
    text-align: center;
}

.jackpot {
    background: green;
    color: white;
}
.lose {
    color: white;
    background: red;
}
```
