# Object Literals

## Table of Contents
- [Shorthand Object Properties](#shorthand-object-properties)
- [Computed Property Names](#comptued-property-names)
- [Shorthand for Adding Methods to Objects](#shorthand-for-adding-methods-to-objects)

## Shorthand Object Properties
- Including a variable in an object literal will automatically assign its value as well.
### Example
```
// Previously
const getStates = (arr) => {
  const max = Math.max(...arr);
  const min = Math.min(...arr);
  const sum = arr.reduce((sum, r) => sum + r);
  const avg = sum / arr.length;
  return { // Returning an object literal containing the variables and their values as key-value pairs.
    max: max,
    min: min,
    sum: sum,
    avg: avg,
  }
}

// Shorthand
const getStates = (arr) => {
  const max = Math.max(...arr);
  const min = Math.min(...arr);
  const sum = arr.reduce((sum, r) => sum + r);
  const avg = sum / arr.length;
  return { // Returning an object literal containing the variables and their values as key-value pairs.
    max,
    min,
    sum,
    avg
  }
}
```

## Computed Property Names
- Improvement for object literal syntax.
- Allows the names of object properties in JS object literal notation to be determined dynamically, i.e., computed.
- We can use a variable as a key name in an object literal property.
- Use when: 
  - creating functions that return objects.
  - adding things (ex: dynamic key) into objects.
### Dynamic Key
- The value of the variable that is in `[]` will become the name of the key.
```
let appSate = "loading";
const app = {
  [appState] = true;
}
app; { loading: true }
```
### Example 1
```
const role = "host";
const person = "Tom Jerry";

// Previously (2 step process)
const cartoon = {};
cartoon[role] = person;
cartoon; // { host: "Tom Jerry" }

// Computed Properties
const cartoon = {
  [role]: person
}
cartoon; // { host: "Tom Jerry" }
```
### Example 2	
```
const state = {
  loading: true,
  name: "",
  job: ""
}

// whatever key that is passed, will be the one that will be updated.
function updateState(obj, key, value) {
  obj[key] = value;
}

updateState(state, "name", "john");
state; // { loading: true, name: "john", job: "" }
```
### Reference
[Dynamic Object Keys](https://www.youtube.com/watch?v=_qxCYtWm0tw)

## Shorthand for Adding Methods to Objects
### Example
```
const math = {
  blah: "Hi!",
  add(x, y) {
    return x + y;
  },
  multiply(x, y) {
    return x * y;
  }
}

math.add(2, 3); // 5
```
