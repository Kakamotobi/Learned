# Virtual DOM

## Table of Contents
- [What is Virtual DOM?](#what-is-virtual-dom)
- [How does it work?](#how-does-it-work)
- [Why Is It Used?](#why-is-it-used)
  - [When Should It Be Used?](#when-should-it-be-used)
- [Critiques](#critiques)
- [Virtual DOM ≠ Shadow DOM](#virtual-dom--shadow-dom)

## What is Virtual DOM?
> A virtual DOM is a lightweight JavaScript representation of the Document Object Model (DOM) used in declarative web frameworks such as React, Vue.js, and Elm. Updating the virtual DOM is comparatively faster than updating the actual DOM (via js). Thus, the framework is free to make unnecessary changes to the virtual DOM relatively cheaply. The framework then finds the differences between the previous virtual DOM and the current one, and only makes the necessary changes to the actual DOM. | Wikipedia

- ***It is a virtual representation of the UI that is kept in memory and synced with the DOM - process called reconciliation.***
  - For example, in React, the ReactDOM library is responsible for this. Based on the state of the UI, ReactDOM will make sure that the DOM is in sync to that state.
  - This allows for a declarative approach as attribute manipulation, event handling, and manual DOM updating is abstracted out (cf. vanilla JS).
- The virtual DOM is a pattern rather than a specific technology/tool.
  - *It is a "blueprint" that contains the details needed to construct/update the real DOM tree.*
- It occurs between the render function being called and the elements actually being rendered on the screen.

## How does it work?
- The framework generates and manages a virtual DOM.
  - The virtual DOM is expressed as JavaScript objects representing the HTML components.
  - The virtual DOM operates in memory.
- A change due to some interaction triggers the entire virtual DOM to update.
  - If the element's property value is changed.
    - Only update the property value.
  - If the element's tag or the component changes.
    - Unmount the corresponding node (in the virtual DOM) and all its nested nodes, and replace with a new virtual DOM.
- Compare the new virtual DOM with a snapshot of the virtual DOM right before the update.
  - This allows the framework to figure out what needs to be updated.
  - The changes are grouped together and applied to the real DOM.
  - Ex: when a prop changes (app's state updated), the framework creates a new virtual DOM object, and reconciles it with the old one to apply necessary changes to the real DOM.
- Apply only the changes to the real DOM.
- The change in the DOM is rendered onto the screen. 
### Notes
- Recalculating and repainting nested/following nodes that do not need updating can be exhaustive if there are many elements. Therefore, key attributes are used.
- Keys are used to match the elements in the pre-updated virtual DOM.
- This allows the framework to identify the unchanged elements and simply add in the new element, effectively preventing unnecessary re-rendering of unchanged components.

## Why Is It Used?
- When a change occurs to the DOM due to some interaction, the render tree is regenerated. This means that the styles of all components are recalculated and go through the layout/reflow process and paint/repaint process in the critical rendering path.
  - With the rise of SPAs and interactivity, it became prevalent and necessary to frequently manipulate the DOM tree. And hence, the need for optimizing this process came to be.
- The real DOM tree for a SPA can be very huge, which can be time consuming to find and update the nodes.
  - The virtual DOM solves this by creating a high level abstraction of the real DOM.
  - Simply use the virtual DOM as a blueprint to determine the update and only make that change.
- Only the changes detected in the virtual DOM are applied to the real DOM. Hence, the entire DOM tree does not have to unnecessarily go through the layout and repaint processes but only the parts that changed.
### When Should It Be Used?
- If building a website with very minimum interaction, directly manipulating the real DOM may perform better.
  - The actual DOM and vanilla DOM manipulation is faster than the virtual DOM because there is no need to calculate the differences before and after (of the virtual DOM).
- If building a website or web app with a lot of interaction requiring a lot of DOM manipulation (Ex: large-scaled and complex SPAs), the virtual DOM can be used to decrease the amount of computations that the browser has to perform, hence, improving performance.

## Critiques
> The virtual DOM operations are *in addition* to the eventual operations on the real DOM. The only way it could be faster is if we were comparing it to a less efficient framework (there wre plenty to go around back in 2013!), or arguing against a straw man ... | Svelte

- Diffing is not free of cost. It would be more efficient to be able to skip the diffing process and go straight into the eventual updating of the real DOM.
  - Although, the diffing algorithms used by frameworks are fast.
- Unnecessary computation and allocation is exhaustive - defaulting to doing unnecessary work.
  - On every change in state, regardless of whether a particular props has changed or not, it is recalculated and reallocating, which is simply overhead.
  - Frameworks are capable of doing this fast enough and there is really no point in optimizing performance.
  - However, it would be faster to not do that in the first place.
  - Defaulting to doing unnecessary work can gradually make the application slow/fail.
- The virtual DOM is not slow or fast. It is usually fast enough.
- Frameworks use the virtual DOM to achieve declarative, state-driven UI development.
  - It allows developers to build applications without thinking about state transitions, with fast enough performance.
  - This means developers are taken burden off of tedious tasks and there are less bugs.
### Svelte
> Svelte is a compiler that knows at *build time* how things could change in your app, rather than waiting to do the work at *run time*. | Svelte

## Virtual DOM ≠ Shadow DOM
- The shadow DOM is a way that browsers can attach hidden, separated DOM trees (starting with a shadow root) to an element (refered to as the shadow host) in the real DOM.
  - **Shadow host:** the real DOM node that the shadow DOM is attached to.
  - **Shadow root:** the root node of the shadow DOM tree.
  - **Shadow tree:** the DOM tree inside the shadow DOM (attached to the shadow root).
  - **Shadow boundary:** the point where the shadow DOM ends and the real DOM begins.
- The shadow DOM can be manipulated the same way as the real DOM nodes.
  - However, the code inside a shadow DOM cannot affect anything outside of it (encapsulation).
- Browsers have been using the shadow DOM as a means to encapsulate the inner structure of an element.
  - This includes variables, styles, etc.
  - For example, in the real DOM we only see the `<video>` element. However, it actually contains buttons and other controls inside its shadow DOM.

## Reference
[Virtual DOM - Wikipedia](https://en.wikipedia.org/wiki/Virtual_DOM)  
[Virtual DOM and Internals - React](https://reactjs.org/docs/faq-internals.html)  
[Virtual DOM is pure overhead - Svelte](https://svelte.dev/blog/virtual-dom-is-pure-overhead)  
[Understanding shadow DOM - Web Components | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)  
