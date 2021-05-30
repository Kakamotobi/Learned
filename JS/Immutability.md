# Immutability in JavaScript
> A mutable object is an object whose state can be modified or changed over time. An immutable object, on the other hand, is an object whose state cannot be modified after it is created.

## Immutable Data Types
- *Because they are primitive value types that are assigned by value and not by reference.*
### Numbers
#### Example 
- 7's value cannot be changed to 8 but the value 7 stored in a variable can be changed to 8.
```
let x = 7;
x += 1;
```
### Strings
#### Example
```
let myString = "I am immutable!";
myString[2] = "S";
console.log(myString) // returns "I am immutable!";
  ```

## Mutable Data Types
- *Because they are reference types that are assigned by reference and not by value.*
### Objects
#### Example
- Any change in the property of x is reflected in the value of y because they share the same reference.
- So, when updating the value of a property of x, the value for all the references to that object is modified.
```
let x = { foo: "bar" };
let y = x;
x.foo = "Something Else";
console.log(y.foo); // "Something Else"
console.log(x === y); // true
```
### Arrays
#### Example
- Both x and y references to the same item, and the method push mutates the original array.
```
let x = ["foo"];
let y = x.push("bar");

console.log(y); // ["foo", "bar"]
console.log(x); // ["foo", "bar"]
console.log(x === y); // true
```
#### Mutating Methods
- `.pop`
- `.push`
- `.shift`
- `.unshift`
- `.sort`
- `.splice`
- `.reverse`

#### Immutating Methods
- Methods that create a new array without mutating the original array.
- `.map`
- `.filter`

## Reference
[Immutability in JavaScript-When and Why Should You Use It](https://dzone.com/articles/immutability-in-javascriptwhy-and-when-should-you#:~:text=A%20mutable%20object%20is%20an,modified%20after%20it%20is%20created.&text=In%20JavaScript%2C%20string%20and%20numbers%20are%20immutable%20data%20types.)  
[Understanding Immutability in JavaScript | CSS-Tricks](https://css-tricks.com/understanding-immutability-in-javascript/)
