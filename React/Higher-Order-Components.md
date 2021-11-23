# Higher Order Components

## Table of Contents
- [React.memo](#reactmemo)

## React.memo
- Memoization
  > ... an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.
- **React will skip rendering the component and reuse the last rendered result if the same props renders the same result as before.**
  - If your function component renders the same result given the same props, the result is memoized. Hence, skipping rerendering and using the remembered old version.
- *For a class-based comopnent, we would use React.PureComponent rather than React.Component to only rerender if the values in the props passed down do change. i.e., if it gets new props, it doesn't automatically rerender unless the props are different.*
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
