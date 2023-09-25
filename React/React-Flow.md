# React Flow

## Table of Contents
- [React Logic](#react-logic)
  - [Rendering Code](#rendering-code)
  - [Event Handlers ft. Effects](#event-handlers-ft-effects)
- [Render and Commit](#render-and-commit)
  - [Trigger a Render](#trigger-a-render)
  - [Render (React)](#render-react)
  - [Commit to the DOM](#commit-to-the-dom)
- [React Lifecycle](#react-lifecycle)
  - [Component Lifecycle](#component-lifecycle)
  - [Effect Lifecycle](#effect-lifecycle)

## React Logic
- There are two types of logic inside React components: **Rendering Code** and **Event Handlers**.
### Rendering Code
- The top-level of a component.
- The phase where props and state are transformed into JSX.
- Must be a pure function that only calculate the result (JSX) and nothing else.
### Event Handlers ft. Effects
- **Effect:** a side effect caused by rendering (in terms of React).
- Effects "do" things rather than calculate.
	- i.e. they perform side effects to change the program's state (usually) based on the user's input.
  - Ex: update an input field, send an API request, animation, connect to a chatroom, etc.
  	- These can't happen during rendering since they are not mere calculations.
- _**Effects run at the end of a commit after the screen/DOM updates.**_
	- Therefore, we can synchronize our components with some external system at this point.
	- Every time the component renders, React will update the screen _and then_ run the code inside `useEffect`.
		- Effects run as a _result_ of rendering.
		- i.e. a piece of code is delayed from running until that render is reflected on the screen.
- _`useEffect`'s clean up function is called each time before the Effect runs again, and also when the component unmounts._
- Effect dependencies are essentially indicators for resynchronization.
- _Each Effect in your code should represent a separate and independent synchronization process._

## Render and Commit
- There are three steps in requesting and serving UI: **Triggering**, **Rendering**, **Committing**.
### Trigger a Render
- Changes to the props, state (including Effect dependencies) trigger a render of a component.
### Render (React)
- Calculate what has changed (to the JSX) since the previous render.
- i.e. call/invoke the function component.
### Commit to the DOM
- If any, apply the detected changes.
### Browser Repaint/Rendering
- The browser renders the DOM.

## React Lifecycle
### Component Lifecycle
- **Mount**
	- The component is added to the screen.
- **Update**
	- The component received new props or state.
- **Unmount**
	- The component is removed from the screen.
### Effect Lifecycle
- An Effect is concerned with how to synchronize an external system to the current props and state.
- **Start synchronizing something**
	- Effect body.
- **Stop synchronizing something**
	- Cleanup function.
- This cycle can happen multiple times depending on the Effect dependencies.
- Thinking in Effects' perspective
	- There is no need to think about Effects in the component lifecycle's perspective (mount, unmount) at all.
	- Rather, simply think about the starting/stopping synchronization.
  > Instead, always focus on a single start/stop cycle at a time. It shouldn’t matter whether a component is mounting, updating, or unmounting. All you need to do is to describe how to start synchronization and how to stop it. If you do it well, your Effect will be resilient to being started and stopped as many times as it’s needed. | react.dev

## Reference
[Synchronizing with Effects – React](https://react.dev/learn/synchronizing-with-effects)  
[Render and Commit - React](https://react.dev/learn/render-and-commit)  
[Lifecycle of Reactive Effects - React](https://react.dev/learn/lifecycle-of-reactive-effects)  
