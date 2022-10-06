# `new`
> …create an instance of a user-defined object type or of one of the built-in object types that has a constructor function. | MDN

- The `new` keyword has nothing to do with class-oriented features.
- A `constructor()` function is an ordinary function that gets automatically called when the `new` keyword is used.
- Therefore, the `new` keyword is merely a Construcion Call of functions.

## The 4 Things that `new` Does
1. Create an empty object.
2. If the prototype is an object, the newly created object's Prototype is set to the constructor function's prototype (otherwise just `Object.prototype`).
3. `this` is bound to the newly created object.
4. Returns that object from the constructor function without having to use the `return` keyword.

## Reference
[new operator - JavaScript MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)  
