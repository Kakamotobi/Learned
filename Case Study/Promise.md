# Promise

## Table of Contents
- [Background](#background)
- [Requirements](#requirements)
- [Finite State](#finite-state)

## Background
- `Promise` is one of a few means that enable us to write asynchronous programming.
- A `Promise` is an object that represents the eventual resolution of success or failure of an asynchronous operation and its resulting value.
  - Callbacks are attached to the `Promise` (via `then()` and `catch()` methods) rather than passing in callback functions, which results in callback hell.

## Requirements
- `constructor(executor)`
  - `executor` = `(resolve, reject) => {}`
    - `resolve(value)`
      - The `Promise` resolves with the passed in `value`.
    - `reject(reason)`
      - The `Promise` rejects with the passed in `reason`.
    - _Note_
      - Errors thrown in the `executor` cause the promise to be rejected.
      - _`fetch` implicitly resolves with the result from the request or rejects with the error._
- `then(onFulfilled, onRejected?)`
  - Collect and attach a callback to run if the `Promise` is fulfilled.
    - Optional: collect and attach a callback to run if the `Promise` is rejected.
  - Return the `Promise` to allow chaining.
- `catch(onRejected)`
  - Collect and attach a callback to run if the `Promise` is rejected.
    - Basically, a shortcut for `onRejected` in `then()`.
  - Return the `Promise` to allow chaining.

## Finite State
<div align="center">
  <img src="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png"  alt="Promise Flow" width="80%" />
</div>

- "PENDING"
  - `resolve()` &rarr; "FULFILLED"
  - `reject()` &rarr; "REJECTED"
- "FULFILLED"
  - async operation &rarr; "PENDING"
- "REJECTED"
  - async operation &rarr; "PENDING"

## Reference
[Promise - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)  
[Promise() constructor - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)  
[Promise.prototype.then() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)  
[Promise.prototype.catch() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)  
