# State Mangement and Data Binding in Web Components

## Table of Contents
- [Objectives](#objectives)
- [Managing State in Simple Components](#managing-state-in-simple-components)
	- [Storing State in Element Instances](#storing-state-in-element-instances)
		- [Improvement](#improvement)
	- [Using the `attributeChangedCallback` Lifecycle](#using-the-attributechangedcallback-lifecycle)
- [Creating an Abstraction Layer for State Management and Binding](#creating-an-abstraction-layer-for-state-management-and-binding)

## Objectives
- Design a unidirectional data flow architecture for state management in Web Components.
- Support data binding in which the View updates the state, which then triggers a rerender of all the View that are using that state.

## Managing State in Simple Components
### Storing State in Element Instances
- Maintaining a state object in the component.
```js
const template = document.createElement("template");
template.innerHTML = `
  <h1>Hello, <span id="name"></span>!</h1>
  <p>You are <span id="age"></span> years old!</p>
  <button class="age-btn younger" type="button">Get younger!</button>
  <button class="age-btn older" type="button">Get older!</button>
`;

class HumanBeing extends HTMLElement {
  constructor() {
    super();
    this.state = {
      name: this.dataset.name,
      age: this.dataset.age,
    };
    const shadowRoot = this.attachShadow({ mode: "open" });
    this.shadowRoot.append(template.content.cloneNode(true));
  }

  connectedCallback() {
    this.render();
    this.shadowRoot.querySelectorAll(".age-btn").forEach((btn) => {
      btn.addEventListener("click", (evt) => {
        const { age } = this.state;
        if (evt.target.classList.contains("younger")) {
          this.setState({ age: parseInt(age) - 1 });
        } else {
          this.setState({ age: parseInt(age) + 1 });
        }
      });
    });
  }

  setState(newState) {
    this.state = { ...this.state, ...newState };
    this.render();
  }

  render() {
    const { name, age } = this.state;
    this.shadowRoot.getElementById("name").innerText = name;
    this.shadowRoot.getElementById("age").innerText = age;
  }
}

customElements.define("human-being", HumanBeing);
```

```html
<human-being data-name="Kakamotobi" data-age="3"></human-being>
```
#### Issues
- We can see that things get complicated and inefficient really quick.
- We had to manually find the DOM nodes that are using any particular state using their `id`s and set them to the current value of their corresponding state in a `render` function, which will be called inside the `setState` function (i.e. whenever the state changes).
- Since the `render()` function is responsible for rendering those DOM nodes, any state change (even if that state is irrelevant to that element), triggers a rerender of all of them.

<div align="center">
	<img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Case%20Study/refImg/case1.png" alt="Case 1" width="60%"/>
</div>

- To solve this, we could loop through every state in `newState` and use type narrowing to call a render for elements that use that state.
#### Improvement
```js
connectedCallback() {
  this.renderEl("name", this.state.name);
  this.renderEl("age", this.state.age);
}

setState(newState) {
  this.state = { ...this.state, ...newState };

  Object.keys(newState).forEach((key) => {
    switch (key) {
      case "name":
        this.renderEl("name", this.state.name);
        break;
      case "age":
        this.renderEl("age", this.state.age);
        break;
      default:
        break;
    }
  });
}

renderEl(id, val) {
  this.shadowRoot.getElementById(id).innerText = val;
}
```

<div align="center">
	<img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Case%20Study/refImg/case2.png" alt="Case 2" width="60%"/>
</div>

##### Issues
- This is very inefficient and unscalable.
- The more complex the component becomes, the more case handling is needed and the more difficult it will be to maintain all the elements that are depending on a particular state.
	- For example, `case "name":` may have to have a long list of renders of individual elements.
### Using the `attributeChangedCallback` Lifecycle
- Instead of maintaining a state object in the component, we could leverage the `attributeChangedCallback` lifecycle instead.
	- Treat each attribute as an individual state.
	- Handle individual renders in the `attributeChangedCallback` lifecycle.
```js
const template = document.createElement("template");
template.innerHTML = `
  <h1>Hello, <span id="name"></span>!</h1>
  <p>You are <span id="age"></span> years old!</p>
  <button class="age-btn younger" type="button">Get younger!</button>
  <button class="age-btn older" type="button">Get older!</button>
`;

class HumanBeing extends HTMLElement {
  constructor() {
    super();
    const shadowRoot = this.attachShadow({ mode: "open" });
    this.shadowRoot.append(template.content.cloneNode(true));
  }

  connectedCallback() {
    this.renderEl("name", this.dataset.name);
    this.renderEl("age", this.dataset.age);

    this.shadowRoot.querySelectorAll(".age-btn").forEach((btn) => {
      btn.addEventListener("click", (evt) => {
        if (evt.target.classList.contains("younger")) {
          this.dataset.age = parseInt(this.dataset.age) - 1;
        } else {
          this.dataset.age = parseInt(this.dataset.age) + 1;
        }
        this.renderEl("age", this.dataset.age);
      });
    });
  }

  static get observedAttributes() {
    return ["name", "age"]
  }

  attributeChangedCallback(name, oldVal, newVal) {
    switch (name) {
      case "name":
        this.renderEl("name", newVal);
        break;
      case "age":
        this.renderEl("age", newVal);
        break;
    }
  }

  renderEl(id, val) {
    this.shadowRoot.getElementById(id).innerText = val;
  }
}

customElements.define("human-being", HumanBeing);
```
#### Issues
- This approach is still prone to the same issues as the previous approach.
- As a component grows, side effects and complexity grows as well in the above approaches.
	- Hence, the need for state management libraries such as "reactive properties" in LitElement library for Web Components.

## Creating an Abstraction Layer for State Management and Binding
