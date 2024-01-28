# React Rendering

## Table of Contents
- [Definition](#definition)
- [When Rendering is Triggered](#when-rendering-is-triggered)
  - [1) Initial Rendering](#1-initial-rendering)
  - [2) Rerendering](#2-rerendering)
- [Rendering Process](#rendering-process)
  - [Overview](#overview)
  - [Render Phase](#render-phase)
  - [Commit Phase](#commit-phase)
  - [Note](#note)

## Definition
- **"Rendering" in React refers to calculating the DOM result (based on the props and states that each component in the tree has) to be rendered by the Browser.**
  - _Separate from the rendering performed by the Browser._
 
## When Rendering is Triggered
- _Note_
  - State used through state management libraries like Redux, MobX, Zustand, etc do not inherently reflect on the DOM. They ultimately have to use one of the below ways to cause rerenders to reflect the state change by using packages like `mobx-react`, `react-redux`.
  - If a state management library doesn't use a separate package, it uses React hooks (Ex: `useState`) internally to trigger rerenders.
### 1) Initial Rendering
- When the user first accesses the application.
### 2) Rerendering
- **Class Components**
  - When `setState` is called.
  - When `forceUpdate` is called.
    - `forceUpdate` ignores `shouldComponentUpdate` and executes the rerender (applies to all descendant components).
- **Function Components**
  - When the state setter (Ex: `setState` from `const [state, setState] = useState()`) is called.
  - When the reducer dispatch (Ex: `dispatch` from `const [state, dispatch] = useReducer(reducer, initialArg, init?)`) is called.
- **Common**
  - When the `key` prop changes.
    - React uses the `key` prop to identify/distinguish between sibling components when rerendering (i.e. when comparing the `current` fiber tree and the `workInProgress` fiber tree).
      - i.e. the `key` allows React to determine whether a particular component refers to the same component in the two trees.
    - Crucial since it is the mechanism in which React can minimize rerendering of components.
    - If no `key` is provided, the `sibling` index in the fiber node is used.
  - When the received props change.
  - When the parent component rerenders.
    - _When a parent component rerenders, children components also rerenders regardless of whether there are actual changes to the children components._

## Rendering Process
### Overview
- Upon triggering a render, React goes through the fiber tree to identify components that need to be updated.
  - If an update is needed, the `render` function is called for class components and the whole function component is called and its result is saved for function components.
- The result from the render outputs a React element, which is just an object describing that particular component, that essentially serve as instructions for React later on.
  - The JSX is transpiled to JavaScript.
  - Example
    ```jsx
    function Component() {
      return (
        <MyComponent a={3} b="hi">
          Hello
        </MyComponent>
      )
    }
    ```
    ```
    // Render Result
    
    { type: MyComponent, props: { a: 3, b: "hi", children: "Hello" }}
    ```
- [Reconciliation](https://github.com/Kakamotobi/Learned/blob/main/React/Virtual-DOM-and-React-Fiber.md#react-fiber---the-internals-of-reacts-vdom) takes place.
  - Each component's render result is collected and then compared to the `current` tree to determine the necessary changes that need to be made to the actual DOM.
- All necessary changes are synchronously applied to the DOM.
- React's rendering process consists of two phases: Render and Commit.
### Render Phase
- Refers to the process of rendering(Ex: `render()` or `return`) each component and comparing them to their previous version in the `current` tree.
- The comparison mainly looks for changes in _type_, _props_, and _key_.
  - Any change in either of these will mark the component as needing update.
- Needs to be pure and has no side effects.
### Commit Phase
- Refers to the process of applying the necessary changes to the actual DOM.
- Once the Commit Phase is done, the Browser begins to render to ultimately display the screen to the user.
- React updates its internal references to the newly created DOM nodes. Then, `componentDidMount` and `componentDidUpdate` lifecycle methods are called in class components, and `useEffect` in function components.
### Note
- Rendering being triggered in React doesn't necessarily mean that DOM updates also occur.
  - If the Render Phase is triggered but no changes were found, React will not continue to the Commit Phase.
- _The Render Phase and Commit Phase used to be executed synchronously_.
  - Therefore, a long rendering process can lead to performance issues.
  - However, with the introduction of the [Fiber architecture](https://github.com/Kakamotobi/Learned/blob/main/React/Virtual-DOM-and-React-Fiber.md#react-fiber---the-internals-of-reacts-vdom) in React 18, asynchronous rendering (concurrency) is adopted.
    - Since the Render Phase is now asynchronous, React can prioritize, stop, restart, drop particular renders.
