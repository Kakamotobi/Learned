# Virtual DOM and React Fiber

## Table of Contents
- [What is Virtual DOM?](#what-is-virtual-dom)
  - [VDOM Background](#vdom-background)
- [How React Works](#how-react-works)
- [React Fiber - The Internals of React's VDOM](#react-fiber---the-internals-of-reacts-vdom)
  - [Fiber](#fiber)
  - [Fiber Tree](#fiber-tree)
  - [Order of Operations (DFS Preorder)](#order-of-operations-dfs-preorder)
- [Critiques](#critiques)
- [Reference](#reference)

## What is Virtual DOM?
> The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. This process is called reconciliation. | React

- Based on the state of the UI, ReactDOM will make sure that the DOM is in sync to that state.
  - This allows for a declarative approach as attribute manipulation, event handling, and manual DOM updating is abstracted out (cf. vanilla JS).
- It occurs between the render function being called and the elements actually being rendered on the screen.
- The essential point is that VDOM allows us to make all the necessary changes behind the scenes in memory, and once its ready, apply the final result to the actual DOM in the browser.
  - The UI is expressed in JS data structures.
### VDOM Background
- Rendering a page is complex and requires expensive operations in the browser's perspective. As websites became more interactive and display data according to these user interactions, the screen needs to update accordingly (i.e. DOM manipulation leading to the [Critical Rendering Path](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Browsers.md#critical-rendering-path)).
  - A change in the DOM due to some interaction leads to the regeneration of the render tree. This means that the styles of all components are recalculated and go through the layout/reflow process and paint/repaint process in the critical rendering path.
  - For example, a change in an element's size leads to a re-Layout, and then a re-Paint, which could be expensive especially when the element has nested child elements.
- One advantage of SPAs is that there is no flickering (for example, when routing to another page). However, it requires a lot of recalculation of the DOM.
- Manually manipulating each of these DOM caculations is also daunting as a developer. We are more interested in the result to be displayed from the interaction rather than all DOM changes from the interaction.
- The VDOM represents the DOM that the browser should render. This is stored and managed in memory. When there is a difference between the VDOM and the actual DOM, only the necessary changes are made to the actual DOM. By executing the calculations within memory instead of the browser, the number of rendering processes that would have been required by the browser can be minimized.
  - Only the changes detected in the VDOM are applied to the real DOM. Hence, the entire DOM tree does not have to unnecessarily go through the layout and repaint processes but only for the parts that changed.
- VDOM is not faster than directly manipulating the DOM. But it is fast enough for most applications.

## How React Works
- When a React application is rendered, the tree (Virtual DOM) representing the application is generated and saved in memory.
  - The Virtual DOM is expressed as JS objects (Fiber Nodes) representing the HTML components.
- This tree is translated into a set of DOM operations and are executed to render the actual DOM by the **Renderer**.
- A change (Ex: through `setState`) due to some interaction regenerates the entire Virtual DOM.
  - If the element's property value is changed.
    - Only update the property value.
  - If the element's tag or the component changes.
    - Remove the corresponding Fiber Node and all of its nested nodes, and replace with a new node.
  - Compare the new Virtual DOM with a snapshot of the Virtual DOM taken right before the update to determine what changes need to be made (**Reconciliation**).
- The changes are grouped together and applied to the actual DOM by the **Renderer**. 
### Notes
- Recalculating and repainting nested/following nodes that do not need updating can be exhaustive if there are many elements. Therefore, key attributes are used.
- Keys are used to match the elements in the pre-updated virtual DOM.
- This allows the framework to identify the unchanged elements and simply add in the new element, effectively preventing unnecessary re-rendering of unchanged components.

## React Fiber - The Internals of React's VDOM
- At its basic, React Fiber is an ordinary object in JS. It is the **Fiber Reconciler**'s job to manage this object (detect any differences with the actual DOM and if there are any, request a rerender of the screen).
- **Reconciliaton** is the process/algorithm that determines what parts of the DOM need to be newly rendered by comparing the VDOM and the actual DOM.
  - Previously, this algorithm was based on a stack; meaning that the necessary operations would take place synchronously.
  - With the introduction of "fiber", this process is now asynchronous.
- Using the information provided by the Reconciler, the **Renderer** actually updates the DOM.
- _Note_
  - _Since the Reconciler and Renderer are implemented separately, React DOM and React Native can use the same Reconciler but use their own Renderers to target different environments._
### Fiber
- The Fiber architecture allows for _incremental rendering_.
  - Incremental rendering refers to being able to split rendering work into chunks and spread it out over multiple frames. It also allows pausing, aborting, or reusing work as new updates come in. It also assigns priority to different types of updates.
- A Fiber Node is essentially a JS object that corresponds to an element in the React component tree.
  - It contains information such as its tag, key, parent node, child node, sibling nodes, props, dependencies, etc.
### Fiber Tree
- Basically a tree of Fiber Nodes.
- There are two Fiber Trees: the Fiber Tree representing the currently rendered UI, and a `workInProgress` Fiber Tree.
- Once the `workInProgress` tree is ready, React simply replaces the current Fiber Tree with the `workInProgress` tree. This is called _double buffering_, and is done in the "commit" phase.
  - Double buffering allows us to make sure all work is done behind the scenes before showing the complete result to the screen at once.
### Order of Operations (DFS Preorder)
#### Fiber & Fiber Tree Flow
- When React works on a Fiber Node, it goes through each child node until it reaches a leaf node.
- It then works on any sibling Fiber Nodes.
- Then, it returns to its parent Fiber Node.
#### Notes
- This process used to be synchronous as it was implemented via a stack. However, now, React is able to prioritize works by pausing, aborting, etc making the process asynchronous.
- For example, animations or input operations can be prioritized.

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

## Reference
[Virtual DOM - Wikipedia](https://en.wikipedia.org/wiki/Virtual_DOM)  
[Virtual DOM and Internals - React](https://reactjs.org/docs/faq-internals.html)  
[Virtual DOM is pure overhead - Svelte](https://svelte.dev/blog/virtual-dom-is-pure-overhead)  
[Understanding shadow DOM - Web Components | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)  
[acdlite/react-fiber-architecture: A description of React's new core algorithm, React Fiber](https://github.com/acdlite/react-fiber-architecture)  
