# Higher Order Components

## Table of Contents
- [React.memo](#reactmemo)

## React.memo
- Memoization
  > ... an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.
- React will skip rendering the component and reuse the last rendered result if the same props renders the same result as before.
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
