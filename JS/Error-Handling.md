# Error Handling
- Recovering from unexpected situations (errors).
- When an error occurs, the following codes in the script will not run.
  - We need to handle that error and continue on with the script.
- "Uncaught" error in the console means that we have not handled the error.
- `console.dir(errorObject)` to get more information about the error.

## `try...catch...finally`
```
try {
  // Run this code.
} catch (err) {
  // Run this code only if the try block throws an error.
} finally {
  // Run this code no matter error or not.
}
```

## Conditional `catch` Blocks
```
try {
  // Some code which may throw three types of exceptions.
} catch (err) {
  if (err instanceof TypeError) {
    // Statements to handle TypeError exceptions.
  } else if (err instanceof RangeError) {
    // Statements to handle RangeError exceptions.
  } else if (err instanceof EvalError) {
    // Statements to handle EvalError exceptions.
  } else {
    // Statements to handle any unspecified exceptions.
    logMyErrors(err); // pass exception object to error handler
  }
}
```

## Reference
[try...catch - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)
