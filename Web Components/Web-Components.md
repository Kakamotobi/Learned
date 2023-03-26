# Web Components

## Table of Contents
- [What is Web Components](#what-is-web-components)
  - [Three Main Technologies of Web Components](#three-main-technologies-of-web-components)
    - [Custom Elements](#custom-elements)
    - [Shadow DOM](#shadow-dom)
    - [HTML Templates](#html-templates)
  - [Life Cycle Methods](#life-cycle-methods)
- [Implementing a Web Component](#implementing-a-web-component)
  - [Example](#example)

## What is Web Components
> Web Components is a suite of different technologies allowing you to create reusable custom elements — with their functionality encapsulated away from the rest of your code — and utilize them in your web apps. | MDN
### Three Main Technologies of Web Components
> ... it consists of three main technologies, which can be used together to create versatile custom elements with encapsulated functionality that can be reused wherever you like without fear of code collisions. | MDN
#### Custom Elements
> A set of JavaScript APIs that allow you to define custom elements and their behavior, which can then be used as desired in your user interface. | MDN
- **CSS Pseudo Classes**
  - **`:defined`**
    - Matches built-in elements and custom elements registered in `CustomElementRegistry`.
  - **`:host`**
    - Selects the Shadow Host of the Shadow DOM.
    - i.e. select the custom element from inside its Shadow DOM.
    - **`:host()`**
      - Selects the Shadow Host of the Shadow DOM only if the specified selector matches the Shadow Host.
      - Ex: `:host(.some-class-name)` selects the Shadow Host if it has a class name of `.some-class-name`.
    - **`:host-context()`**
      - Selects the Shadow Host of the Shadow DOM only if the specified selector matches the Shadow Host's ancestors.
      - i.e. the custom element or elements within the current element's Shadow DOM can apply different styles based on their position within the regular DOM or based on classes/attributes that are applied to ancestor elements.
#### [Shadow DOM](https://github.com/Kakamotobi/Learned/main/DOM/Shadow-DOM.md)
> A set of JavaScript APIs for attaching an encapsulated "shadow" DOM tree to an element — which is rendered separately from the main document DOM — and controlling associated functionality. In this way, you can keep an element's features private, so they can be scripted and styled without the fear of collision with other parts of the document. | MDN  

- A mechanism to attach hidden DOM trees to elements in the regular DOM tree.
#### HTML Templates
> The `<template>` and `<slot>` elements enable you to write markup templates that are not displayed in the rendered page. These can then be reused multiple times as the basis of a custom element's structure. | MDN  
- _HTML Templates are used as the content of the Shadow DOM of web components._
  - Styles that are applied to the template's contents are encapsulated inside the custom element.
- **`<template>`**
  > The `<template>` HTML element is a mechanism for holding HTML that is not to be rendered immediately when a page is loaded but may be instantiated subsequently during runtime using JavaScript. | MDN  
  - _The `<template>` element and its contents are not rendered on the screen until appended to a document._
  - **HTMLTemplateElement.content**
    > A read-only `DocumentFragment` which contains the DOM subtree representing the `<template>` element's template contents. | MDN  
    - **`DocumentFragment`**
      - An interface/node that represents a document object with no parent initially.
        - i.e. a document fragment is not part of the document tree structure until it is added in manually. Therefore, any changes made to a document fragment does not affect the document.
      - A lightweight version of `Document`.
      - Since it is inserted once, it only requires one reflow and render.
        - cf. directly manipulating the DOM requires that many interactions.
      - cf. when manipulating the regular DOM, it could trigger a reflow and render for each node that was added.
      - Use Case: improve performance when manipulating the DOM
        - When heavy DOM manipulation is required.
        - When you need to dynamically append items in loops.
- `Node.cloneNode()`
  - Returns a copy of the `Node`.
  > Cloning a node copies all of its attributes and their values, including intrinsic (inline) listeners. It does not copy event listeners added using addEventListener() or those assigned to element properties (e.g., node.onclick = someFunction) | MDN  
  - cf. `Document.importNode()`
  - Use to clone a `Node` or `DocumentFragment` from another document and later insert into the current document (by, for example, using `appendChild`).
### Life Cycle Methods
- `connectedCallback`
  - Runs when the custom element connects to the DOM for the first time.
- `disconnectedCallback`
  - Runs when the custom element disconnects from the DOM.
- `adoptedCallback`
  - Runs when the custom element is moved to another document.
- `attributeChangedCallback`
  - Runs when an attribute of the custom element is added, removed, or changed.
  - Use the `observedAttributes` static getter to specify the attributes to observe and call `attributeChangedCallback` for.
    ```js
    static get observedAttributes() {
      return [/* array of attribute names to monitor for changes */];
    }
    
    attributeChangedCallback(name, oldValue, newValue) {
      // called when one of attributes listed above is modified
    }
    ```

## Implementing a Web Component
1. Use the `class` syntax to specify the component(custom element)'s functionality.
2. Register the custom element using `CustomElementRegister.define()`.
3. Attach a Shadow DOM to the custom element using `Element.attachShadow()`.
    - Child elements and event listeners can be added to the Shadow DOM.
4. Define an HTML template using `<template>` (and `<slot>`), which will be cloned and attached to the Shadow DOM.
5. The custom element can be used like built-in HTML elements.
### Example
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

## Reference
[Web Components | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components)  
[`<template>`: The Content Template element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)  
[HTMLTemplateElement - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTemplateElement)  
[DocumentFragment - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)  
[Why You Should Use Docment Fragments - Webtips](https://www.webtips.dev/document-fragments)  
[Node.cloneNode() - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode)  
[Custom elements](https://javascript.info/custom-elements)  
[Open Web Components](https://open-wc.org/)  
[Lit](https://lit.dev/)  
