# Array-like Objects
- An object which has a length property of a non-negative integer, and properties indexed from zero, but doesn't have built-in array methods such as `map()`.

## Examples
- HTMLCollection
  - `document.forms`
- DOMTokenList
  - `Element.classList`
- NodeList
  - `Node.chidlNodes`
  - `document.querySelectorAll()`
  - *Not an array but it is possible to iterate over it with `forEach()*

## Converting Array-like Objects to Array
- `Array.prototype.slice.call(arrayLike)`
  - `.call()` method calls a function with a given `this` value and arguments provided individually.
- `Array.from(arrayLike)`
  - Creates a new, shallow-copied array instance from an array-like or iterable object.
- `[...arrayLike]`
