# Data Types

## Value Type
- When we create a variable to store a ***primitive*** type, we’re creating a value type variable.
  - Every variable that is made is stored in JS memory, and when working with a value type variable, the actual value is stored in memory.
  - Ex: String, Number, Boolean, undefined, null, etc.
### Example
```js
// Value Types
let fruit = "orange";
let color = fruit;

console.log(fruit); // "orange"
console.log(color); // "orange"

fruit = "watermelon";

console.log(fruit); // "watermelon"
console.log(color); // "orange"
```

## Reference Type
- However, for large things like arrays and objects, instead of storing all the variables in the array/object itself in memory, JS stores a reference to that array/object.
  - The variable set to the array/object doesn’t hold the array/object. It holds a reference to where the array/object is in memory.
### Example
```js
const veggies = ["cucumber", "carrot", "onion"]; // can use const since it is the reference type that will stay constant, not the values.
const groceryList = veggies;

console.log(veggies); // ["cucumber", "carrot", "onion"]
console.log(groceryList); // ["cucumber", "carrot", "onion"]

veggies.push("spinach");

console.log(veggies); // ["cucumber", "carrot", "onion", "spinach"]
console.log(groceryList); // ["cucumber", "carrot", "onion", "spinach"]
```
