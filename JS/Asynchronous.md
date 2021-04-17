# Asynchronous JS

## Prerequisites
### Single Threaded and Synchronous
- JS is **always** single threaded and synchronous.
- Single Threaded
  - At any given point in time, a single JS thread is running.
  - i.e., only one thing happens at a time in the JS script.
- Synchronous
  - Everything is executed one by one.
  - i.e., if one task takes 5s, the following code will have to wait their turn for 5s.
  - Ex: HTTP request.
    - Nothing happens until the response gets back (blocking, a.k.a. beach ball).

### Call Stack
- The JS Engine creates a **stack** data structure where function execution contexts are placed when the function is called or invoked.
- Mechanism that the JS interpreter uses to keep track of its place in a script and what's being executed.
- **Stack**
  - LIFO/FILO data structure.
- Basic Flow
  - Script calls a function, which is added to the call stack.
  - Any other functions that are then called by that function are added to the call stack further up, and run where their calls are reached.
  - When the current function is finished, it is taken off the stack and resumes execution wher it left off in the last code listing.

## Working Around JS' Synchronous Nature
For example, when making requests to servers, it can take some time to get the data but we don't want our program/website to stall and wait for the data to come back. We want to keep executing our script.

*JS is only asynchronous in that it can hand off certain tasks to the browser, AJAX, etc. to handle while it runs through the script.*

### Web APIs
- **`setTimeout()`**
  - Sets a timer which executes a function or specified code once the timer expires.
  - Telling the browser: "when the timer ends, push the callback function to the callback queue."
  - Flow:
    - JS runs through script (synchronously).
    - When JS encounters a Web API function, it is passed on to the browser (asynchronous bit).
      - "Hey browser, please remind me to put this code back into my call stack after 3s."
    - In the meantime, JS continues to run through the script (synchronously).
    - Browser finishes the given task (placing and checking a timer).
      - "Hey JS, 3s are over. I'm leaving this code in the callback queue for you to take and put in your callstack asap."
    - The callback is returned to the stack and JS continues to run through the script (synchronously).
- **`setInterval()`**
  - Repeatedly calls a function or specified code with a fixed time interval between each call.
- **`clearInterval()`**
  - Cancels interval set by `setInterval`.
  - Syntax: `clearInterval(IntervalID)`
- More on APIs [here](Web-APIs.md)

## Dealing with Asynchronous Data

### Callbacks

### Promises
#### Creating Promises
#### Working with Promises

### async keyword
- Dealing with promises/response but in a more elegant way.
- Looks more like synchronous programming. Rather than when using the .then(), .catch().

### await keyword

### AJAX

## Reference
[Asynchronous JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
