# Shadow DOM

## Table of Contents
- [What is Shadow DOM?](#what-is-shadow-dom)
  - [Terminologies](#terminologies)
- [Creating a Shadow DOM](#creating-a-shadow-dom)
- [Shadow DOM and Web Components](#shadow-dom-and-web-components)
  - [Example - Shadow DOM](#example---shadow-dom)
  - [Example - Using `<template>`](#example---using-template)
- [Styling Elements in the Shadow DOM](#styling-elements-in-the-shadow-dom)

## What is Shadow DOM?
> An important aspect of web components is encapsulation — being able to keep the markup structure, style, and behavior hidden and separate from other code on the page so that different parts do not clash, and the code can be kept nice and clean. The Shadow DOM API is a key part of this, providing a way to attach a hidden separated DOM to an element. | MDN

- The Shadow DOM is much like the ordinary "light" DOM but different in the way it is generated and used in relation to other elements on the document.
- DOM elements come together to compose a DOM tree.
  - The Shadow DOM is essentially an isolated and enclosed tree in the DOM.

<p align="center">
  <img src="https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM/shadowdom.svg" alt="Shadow Tree" width="80%" />
</p>

### Terminologies
- **Shadow Host**
  - The ordinary node (in the "light" DOM) to which the Shadow DOM is added.
  - Not all elements can be a Shadow Host.
    - Ex: `<a>`, `<img>`.
- **Shadow Tree**
  - The DOM tree inside the Shadow DOM.
- **Shadow Boundary**
  - The point where the Shadow DOM ends and the "light" DOM resumes.
- **Shadow Root**
  - The root node of the Shadow Tree.
  - If the encapsulation mode (`mode`) is set to `open`, the Shadow Root can be accessed outside of the Shadow DOM.

## Creating a Shadow DOM
```js
const div = document.createElement("div");
const shadowRoot = div.attachShadow({ mode: "open" }); // `div` is the Shadow Host.
const p = document.createElement("p");
p.innerText = "I am in the Shadow DOM";
shadowRoot.appendChild(p); // `p` is added to the Shadow DOM
```
```html
<div>
  #shadow-root (open)
  <p>I am in the Shadow DOM</p>
</div>
```
```
document
  shadow host
    shadow root
      p
        I am in the Shadow DOM
```

## Shadow DOM and Web Components
- The Shadow DOM API is included in the Web Components standard.
- A Shadow DOM is often created for the custom element that you created.
### Example - Shadow DOM
```js
class MyElement extends HTMLElement {
  constructor() {
    super();
    const shadowRoot = this.attachShadow({ mode: "open" });
    shadowRoot.innerHTML = `
      <h1>Shadow DOM</h1>
      <p>I am in the Shadow DOM</p>
      <style>p { color: blue; }</style>
    `;
  }
}

customElements.define("my-element", MyElement);
```
```html
<my-element>
  #shadow-root (open)
  <h1>Shadow DOM</h1>
  <p>I am in the Shadow DOM</p>
  <style>p { color: blue; }</style>
</my-element>
```
```
document
  my-element
    shadow root
      h1
        Shadow DOM
      p
        I am in the Shadow DOM
      style
        color: blue
```
### Example - Using `<template>`
```js
const template = document.createElement("template");
template.innerHTML = `
  <h1>Shadow DOM</h1>
  <p>I am in the Shadow DOM</p>
  <style>p { color: blue; }</style>
`;

class MyElement extends HTMLElement {
  constructor() {
    super();
    const shadowRoot = this.attachShadow({ mode: "open" });
    shadowRoot.append(template.content.cloneNode(true));
  }
}

customElements.define("my-element", MyElement);
````

## Styling Elements in the Shadow DOM
- [Refer 1](https://stackoverflow.com/questions/61626493/slotted-css-selector-for-nested-children-in-shadowdom-slot)
- [Refer 2](https://css-tricks.com/styling-in-the-shadow-dom-with-css-shadow-parts/)
- [Refer 3](https://blog.jiayihu.net/css-resets-in-shadow-dom/)

## Reference
[Understanding Shadow DOM in Web Components - Ultimate Courses](https://ultimatecourses.com/blog/understanding-shadow-dom-in-web-components)  
[Using Shadow DOM - Web Components | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)  
[Learn Web Components In 25 Minutes - YouTube](https://www.youtube.com/watch?v=2I7uX8m0Ta0&ab_channel=WebDevSimplified)  
[Shadow DOM slots, composition](https://javascript.info/slots-composition)  
[::slotted CSS selector for nested children in shadowDOM slot - Stack Overflow](https://stackoverflow.com/questions/61626493/slotted-css-selector-for-nested-children-in-shadowdom-slot)  
