# The Event Loop and Call Stack

## Table of Contents
- [The Premise](#the-premise)
- [Case Example](#case-example)
  - [What I Expected](#what-i-expected)
  - [Output](#output)

## The Premise
- The Event Loop takes the first callback from the Callback Queue when the Call Stack is empty.
- **So, when is the Call Stack empty?**

## Case Example
```js
const data = [1, 2, 3, 4, 5, 6, 100];

function asyncRun(arr, fn) {
  arr.forEach((v, i) => {
    setTimeout(() => fn(i), 0);
  });
};

function sync1() {
  data.forEach((v, i) => {
    console.log("sync1 -", i);
  });
}

function sync2() {
  data.forEach((v, i) => {
    console.log("sync2 -", i);
  });
}

asyncRun(data, (idx) => console.log("async -", idx));
sync1();
for (let i = 0; i < 5000000000; i++) {} // approx. 5 seconds to make sure `setTimeout`s' callbacks are ready to be pushed to the call stack.
// At this point, the Call stack is presumably empty and there are callbacks waiting in the Callback Queue.
sync2();
```
### What I Expected
- Since `sync1()` is popped off the Call Stack, and the `for` loop doesn't push anything onto the Call Stack (just eats some time), the Call Stack must be empty right?
- At this point, I expected the Event Loop to reach the Callback Queue and push the first callback onto the Call Stack.
```
sync1 - 0
sync1 - 1
sync1 - 2
async - 0
async - 1
async - 2
sync2 - 0
sync2 - 1
sync2 - 2
```
### Output
- The Event Loop did not retrieve a callback from the Callback Queue and push it onto the Call Stack.
- This is because **the Global Execution Context is still on the Call Stack**.
  - Therefore, the program will continue executing until the the last line.
  - At this point, the Global Execution Context will be removed from the Call Stack, and the Event Loop will check the Callback Queue.
  - i.e. all the synchronous code will be executed first before the the Event Loop begins to execute the (async) callbacks in the Callback Queue.
```
sync1 - 0
sync1 - 1
sync1 - 2
sync2 - 0
sync2 - 1
sync2 - 2
async - 0
async - 1
async - 2
```
