# `new`
> …create an instance of a user-defined object type or of one of the built-in object types that has a constructor function. | MDN

## The 4 Things that `new` Does
1. Create an empty object.
2. Links (sets the constructor of) this object to another object (now we can add methods to the prototype and not to all instances).
3. Sets `this` keyword to point to that new object.
4. Returns that object from the constructor function without having to use the `return` keyword.

## Reference
[new operator - JavaScript MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)  
