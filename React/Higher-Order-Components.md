# Higher Order Components

## Table of Contents
- [`withRouter`](#withrouter)
- [`React.memo`](#reactmemo)

## `withRouter`
- react-router-dom HOC.
- [Check here](https://github.com/Kakamotobi/Learned/blob/main/React/22-React-Router-Patterns.md#withrouter-higher-order-component)

## `React.memo`
- Memoization
  > ... an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.
- **React will skip rendering the component and reuse the last rendered result if the same props renders the same result as before.**
  - If your function component renders the same result given the same props, the result is memoized. Hence, skipping rerendering and using the remembered old version.
- Does a shallow comparison of props.
  - You can provide a custom comparison function as the second argument for more control over the comparison.
- *For a class-based component, we would use React.PureComponent rather than React.Component to only rerender if the values in the props passed down do change. i.e., if it gets new props, it doesn't automatically rerender unless the props are different.*
- *Note*
  - React.memo only checks for prop changes. So, if your function component wrapped in React.memo has a `useState`, `useReducer`, or `useContext` Hook in its implementation, the component will still rerender when state or context change.
  - React.memo is used for performance optimization, not to "prevent" a render from happening.
### Syntax
```js
function MyComponent(props) {
  /* render using props */
}

function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}

export default React.memo(MyComponent, areEqual);
```


## Reference
[Memoization](https://en.wikipedia.org/wiki/Memoization)  
[React.memo](https://reactjs.org/docs/react-api.html#reactmemo)  
