# Iteration Protocols

## Table of Contents
- [Iterable Protocol](#iterable-protocol)
- [Iterator Protocol](#iterator-protocol)
- [Example - creating a custom iterable](#example---creating-a-custom-iterable)
- [Synchronous Iteration](#synchronous-iteration)
- [Asynchronous Iteration](#asynchronous-iteration)
  - [`for await...of`](#for-awaitof)

## Iterable Protocol
> The iterable protocol allows JavaScript objects to define or customize their iteration behavior, such as what values are looped over in a `for...of` construct. | MDN

- For an object to be an **iterable**, it must implement the `@@iterator` method.
  - _i.e. one of the objects along its prototype chain must have a property with a `@@iterator` key._
  - This `@@iterator` method can be accessed through the `Symbol.iterator` constant on the object.
    ```js
    const iteratorObj = "IAmIterable"[Symbol.iterator]();
    ```
   - When beginning iteration (Ex: `for...of` loop), the iterable's `@@iterator` method is invoked.
   - The returned iterator is then used to get the values of the iterable that is to be iterated.
- Some built-in types that are iterables:
  - `Array`, `Map`.
    - _Note: `Object` is not an iterable._

## Iterator Protocol
> The iterator protocol defines a standard way to produce a sequence of values (either finite or infinite), and potentially a return value when all values have been generated. | MDN

- i.e. defines how a sequence of values should be produced from an object.
- For an object to be an **iterator**, it must implement the `next()` method.
  - Returns an object of the `IteratorResult` interface.
    - The `IteratorResult` interface consists of `value` and `done`.

## Example - creating a custom iterable
```js
const countMax10 = {};

// Custom `@@iterator` that iterates over values 1 through 10.
countMax10[Symbol.iterator] = function() {
  let count = 0;
  done = false;
  return {
    next() {
      count++;
      if (count === 10) done = true;
      return { value: count, done };
    }
  };
}

for (const count of countMax10) {}
```
```js
const iterator = countMax10[Symbol.iterator]();

while (true) {
  const res = iterator.next();
  if (result.done) break;
}
```

## Synchronous Iteration
```js
const syncIterable = "IAmSyncIterable";
const syncIterator = syncIterable[Symbol.iterator](); // @@iterator method

syncIterator.next(); // { value: "I", done: false }
syncIterator.next(); // { value: "A", done: false }
syncIterator.next(); // { value: "m", done: false }
```

## Asynchronous Iteration
- `AsyncIterator.next()` returns a Promise of `IteratorResult` interface (`{ value, done }`).
```js
const asyncIterable = createAsyncIterable("IAmAsyncIterable");
const asyncIterator = asyncIterable[Symbol.asyncIterator]();

asyncIterator.next()
  .then((iteration1Res) => {
    console.log(iteration1Res); // { value: "I", done: false }
    return asyncIterator.next();
  })
  .then((iteration2Res) => {
    console.log(iteration2Res); // { value: "A", done: false }
    return asyncIterator.next();
  })
  .then((iteration3Res) => {
    console.log(iteration3Res); // { value: "m", done: true }
    return;
  });

// OR

console.log(await asyncIterator.next()); // { value: "I", done: false }
console.log(await asyncIterator.next()); // { value: "A", done: false }
console.log(await asyncIterator.next()); // { value: "m", done: true }
```
### `for await...of`
- An asynchronous version of `for...of`.
```js
for await (const el of asyncIterable) {}
```

## Reference
[Iteration protocols - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)  
[JavaScript Iterables](https://www.w3schools.com/js/js_object_iterables.asp)  
[ES2018: asynchronous iteration](https://2ality.com/2016/10/asynchronous-iteration.html)  
[javascript - Using async/await with a forEach loop - Stack Overflow](https://stackoverflow.com/questions/37576685/using-async-await-with-a-foreach-loop)  
