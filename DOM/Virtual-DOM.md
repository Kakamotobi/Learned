# Virtual DOM

## Table of Contents
- [What is Virtual DOM?](#what-is-virtual-dom)
- [How it Works](#how-it-works)
  - [Reconciliation](#reconciliation)

## What is Virtual DOM?
> The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. This process is called reconciliation. | React

> This approach enables the declarative API of React: You tell React what state you want the UI to be in, and it makes sure the DOM matches that state. This abstracts out the attribute manipulation, event handling, and manual DOM updating that you would otherwise have to use to build your app. | React

- "Virtual DOM" is not a specific technology that exists. It is rather a concept/pattern.
- **Virtual DOM is a mechanism to detect changes that need to be applied to the DOM and to effectively minimize the number of actual DOM manipulations.**
- React.js was the library to popularize the term "Virtual DOM".
  - Other libraries like Vue.js also use Virtual DOM.

This virtual DOM node is used by React to optimize updates and minimize the number of actual DOM manipulations.


## How it Works
- The JSX that a React component returns is used to create a Virtual DOM node of that component.
- Each time its state changes, a new Virtual DOM node is generated.
### Reconciliation
- Reconciliation refers to React detecting whether a rerender on the actual DOM is needed by comparing the previous Virtual DOM and the current Virtual DOM.
- Any discrepancies between the before/after Virtual DOMs are applied to the actual DOM.
- React has to go through this process everytime a state changes in the component.
  - This is because React does not have any understanding of the values flowing through the application. It just treats it like a black box.

<div align="center">
  <img src="https://blog.logrocket.com/wp-content/uploads/2023/01/5-react-actual-dom-update-repaint.png" alt="Virtual DOM Reconciliation" width="80%" />
</div>

#### The Process
- Check the element.
  - Ex: is it still a `div`?
- Check the attributes.
  - Ex: does it still have the same class name?
- Check the text, value, etc.
  - Ex: does it still have the same value?
- Check its children.

## Reference
[Virtual DOM and Internals – React](https://legacy.reactjs.org/docs/faq-internals.html)  
[What is the virtual DOM in React? - LogRocket Blog](https://blog.logrocket.com/virtual-dom-react/)  


