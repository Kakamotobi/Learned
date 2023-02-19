# Asynchronous JS

## Table of Contents
- [Prerequisites](#prerequisites)
- [Working Around Synchronous JS](#working-around-synchronous-js)
  - [Async in JS Example](#async-in-js-example)
  - [Some Browser APIs](#some-browser-apis)
  - [The Event Loop](#the-event-loop)
    - [Node.js & libuv Library](#nodejs--libuv-library)
      - ["Blocking" in Node.js](#blocking-in-nodejs)
      - [libuv Library](#libuv-library)
      - [Phases of the Event Loop](#phases-of-the-event-loop)
      - [`process.nextTick(callback)`](#processnexttickcallback)
      - [Event Emitter](#event-emitter)
- [Dealing with Asynchronous Data](#dealing-with-asynchronous-data)
  - [Option 1 - Callbacks](#option-1---callbacks)
  - [Option 2 - Promises](#option-2---promises)
  - [Option 3 - Async/Await Functions](#option-3---asyncawait-functions)
- [AJAX and Web APIs](#ajax-and-web-apis)
  - [HTTP Request and Response](#http-request-and-response)
  - [Sending HTTP Requests via JS](#sending-http-requests-via-js)
    - [Option 1 - Making XHRs](#option-1---making-xhrs)
    - [Option 2 - Fetch API](#option-2---fetch-api)
    - [Option 3 - AXIOS](#option-3---axios)
- [Reference](#reference)

## Prerequisites
- [Asynchronous Programming](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Asynchrony.md)
- [JS is Single-threaded and Synchronous](https://github.com/Kakamotobi/Learned/blob/main/JS/JavaScript.md#about-javascript)
- [JavaScript Runtime Environment](https://github.com/Kakamotobi/Learned/blob/main/JS/JavaScript.md#javascript-runtime-environment)

## [Working Around Synchronous JS](https://github.com/Kakamotobi/Learned/blob/main/JS/JavaScript.md#javascript-runtime-environment)
- _JS is is **single-threaded** and **synchronous** by default._
  - i.e. JS cannot create new threads and cannot run in parallel.
- _JS is only **asynchronous** in that it can hand off certain tasks to the Browser/Runtime(Ex: Node.js) APIs while it synchronously runs through the script._
  - i.e. JS offloads async operations to the system kernel.
  - JS can perform these async operations through the use of an [**Event Loop**](https://github.com/Kakamotobi/Learned/main/JS/Asynchronous/Async.md#the-event-loop).
    - On the Web, browsers provide the event loop.
    - Node uses the libuv library for the event loop.
    - **_Note:_** _the event loop is not an inherent part of the JS engine. It is a component of the runtime environment._
      - i.e. the libuv library is just another dependency of Node.js as is V8.
- Since the main thread is responsible for rendering views, JS is designed to be non-blocking.
- And since JS is synchronous and makes use of an event loop for asynchronous operations, JS is constrained to use **callbacks**.
  - Since callbacks are functions, there are two options to manage state:
    - Pass in variables directly to the function.
    - Retrieve a variable from a cache, session, file, DB, network, etc.
  - This is why [**closures**](https://github.com/Kakamotobi/Learned/blob/main/JS/Scope.md#closure) play an important role in async operations in JS.
### Async in JS Example
- What happens if something takes a long time?
  - For example, when making requests to servers, it can take some time to get the data but we don't want our program/website to stall and wait for the response. We want to keep executing our script.
  - Example
    ```js
    const newTodo = input.value; // get user input
    saveToDatabase(newTodo); // this could take a while
    input.value = ""; // reset form input
    ```
- For processes that take a long time, we pass a callback function, which will be executed at the appropriate time after the time has passed.
  - **_The callback function that has been passed in is what we want the browser to remind JS of._**
    - Ex: “Hey browser, set this timer for me and **when you’re done, give me this callback back.**”
    - Hence, JS is not hung up on the async operation, until the browser returns the callback to JS.
    - This is why callbacks are used frequently (for things that require asynchronous functions, which take time).
  - Example
    ```js
    console.log("I print first!");
    setTimeout(() => {
      console.log("I print after approx. 3s");
    }, 3000);
    console.log("I print second!");
    ```
    - JS runs through the script (synchronously).
    - When JS encounters `setTimeout`, it passes it on to the browser (asynchronous bit) along with the callback.
      - “Hey browser, remind me to put this callback back into my call stack after 3s.”
    - In the meantime, JS continues to run through the script (synchronously).
    - The browser finishes the given task (placing and checking a timer).
      - “Hey JS, 3s are over. I’m leaving this code in the callback queue for you to take and put in your callstack asap.”
    - The browser returns the callback to the stack and JS continues to run through the script (synchronously).
### Some Browser APIs
- **`setTimeout()`**
  - Sets a timer which executes a function or specified code once the timer expires.
  - Telling the browser: "when the timer ends, push the callback function to the callback queue."
- **`setInterval()`**
  - Repeatedly calls a function or specified code with a fixed time interval between each call.
- **`clearInterval()`**
  - Cancels interval set by `setInterval`.
  - Syntax: `clearInterval(IntervalID)`
- More on APIs [here](../Web-APIs.md).
### The Event Loop
- **The event loop continuously checks the queues for any new messages, and executes the corresponding event handlers for each message.**
- It is called the "event loop" because it usually runs forever.
- The event loop itself is _single-threaded_ and operates _synchronously_.
- As a single-threaded language, concurrency in JS refers to the event loop executing JS callbacks after completing other work.
- Unlike event loops of other programming languages, the event loop for JS does not allow the creation of additional threads to handle concurrent work.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/JS/refImg/js-runtime-environment.png" alt="JS Runtime Environment" width="80%"/>
</p>

#### Event Loop Flow
- Code is added to the call stack.
- If code is synchronous, it will execute and immediately return. If code is asynchronous, it will offload it to the browser or libuv API.
- When the browser or libuv is done, it will send the event (primarily a callback) to the event queue.
- If the call stack is empty, the event loop takes the first callback in the event queue and puts it in the call stack.
- Example
  ```js
  const startTime = Date.now();
  setTimeout(() => console.log(`I was placed in the callback queue after 2000ms, but I executed after ${Date.now() - startTime}ms.`), 2000);
  while (Date.now() - startTime < 5000) {}
  ```
  ```
  I was placed in the callback queue after 2000ms, but I executed after 5000ms.
  ```
#### Node.js & libuv Library
<p align="center">
  <img src="https://nexocode.com/images/event-loop-nodejs-architecture1.webp" alt="Node.js Architecture" width="80%"/>
</p>

##### "Blocking" in Node.js
- "Blocking" refers to when the Node.js process has to wait for a non-JS operation (Ex: I/O) to complete; since the Event Loop cannot continue executing JS.
  - cf. poor performing code in JS is not referred to as "blocking".
- Blocking operations include the synchronous methods in the Node.js standard library that use libuv, and some native modules.
- The Node.js standard library provides **non-blocking asynchronous** and **blocking synchronous** versions of all I/O methods that accept callbacks.
##### libuv Library
> libuv enforces an **asynchronous**, **event-driven** style of programming. Its core job is to provide an event loop and callback based notifications of I/O and other activities. libuv offers core utilities like timers, non-blocking networking support, asynchronous file system access, child processes and more. | libuv
##### Phases of the Event Loop
- **_Each phase has a queue of callbacks that it needs to execute._**
- The event loop moves to the next phase if the the queue has been exhausted or the callback limit is reached.
- Once the event loop runs through, if there aren't any async I/Os or timers that are waiting, Node.js stops the event loop.
  - **Async I/O** refers to the program's interaction with the system disk (i.e. reading/writing files) or network operations.
- **Flow**
  - Node.js starts up.
    - The event loop is initialized.
  - It starts going through the input script.
  - Events are added to the queues accordingly.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/JS/refImg/phases-of-event-loop.png" alt="Phases of the Event Loop" width="80%"/>
</p>

- **Timers**
  - Execute callbacks of `setTimeout()` and `setInterval()`.
- **Pending Callbacks**
  - Execute callbacks related to I/O.
  - Ex: callback for when a file reading is done.
  - Ex: callback for when network operation is done.
- **Idle, Prepare**
  - Only used internally.
- **Poll**
  - Retrieve new I/O events, and tries to execute their callbacks immediately. If not, the event is registered as a pending callback.
  - Therefore, it decides when the next I/O polling will be and process events in the polling queue.
  - This is kind of like the entry point.
- **Check**
  - Execute callbacks of `setImmediate()`.
- **Close Callbacks**
  - Close as in "ending", not distance.
  - Ex: `socket.on("close", ...)`
##### `process.nextTick(callback)`
- When the Event Loop goes around once, it is called a **tick**.
- **The JS engine invokes the callback at the end of the current operation (before the next tick starts).**
- _Therefore, it tells the JS engine to process this callback as soon as the current function ends; and not queue it._
  - i.e. it is a way to ensure that this callback is executed before the next event loop iteration.
  - cf. in `setTimeout(callback)`, the callback is executed at the end of the next tick (not before the next tick starts like `process.nextTick()`).
- Example
  ```js
  console.log("I print first");
  
  setTimeout(() => console.log("I print fourth"));
  
  setImmediate(() => console.log("I print third"));
  
  process.nextTick(() => console.log("I print second"));
  ```
##### Event Emitter
- Node.js' events module offers the `EventEmitter` class, which can be used to handle events.
###### Example
```js
import EventEmitter from "events";

const eventEmitter = new EventEmitter();

// The `on` method registers a callback to be executed when a particular event ("something") is triggered.
const cb = (arg1, arg2) => console.log("something");
eventEmitter.on("something", cb);

// The `emit` method triggers an event (the "something" event).
eventEmitter.emit("something", "argument1", "argument2");

// The `once` method registers an event listener that activates just once.
eventEmitter.once("blah", () => console.log("I only activate once"));

// The `removeListener()`/`off()` method removes the specified listener from the specified event.
eventEmitter.removeListener("something", cb);

// The `removeAllListeners()` method removes all listeners from the specified event.
eventEmitter.removeAllListeners("blah");
```

## Dealing with Asynchronous Data
### Option 1 - Callbacks
#### Definition
- A function that is passed as an argument to another function, which will invoke the argument(callback) at a given time to complete some kind of routine or action.
- Async callbacks are functions that are specified as arguments when calling a function which will start executing code in the background.
#### Example
```js
// THE CALLBACK VERSION

const fakeRequestCallback = (url, successCB, failureCB) => {
  const delay = Math.floor(Math.random() * 4500) + 500;
  setTimeout(() => {
    if (delay > 4000) {
      // message is passed to the failure callback if it is executed.
      failureCB("Connection Timeout :(");
    } else {
      // message is passed to the success callback if it is executed.
      successCB(`Here is your fake data from ${url}`);
    }
  }, delay);
};
```
- A function for async operations usually requires two or more callbacks (one for success, one for failure) as its arguments.
```js
fakeRequestCallback(
  "books.com/page1",
  function (response) {
    console.log("IT WORKED!!!!");
    console.log(response);
    fakeRequestCallback(
      "books.com/page2",
      function (response) {
        console.log("IT WORKED AGAIN!!!!");
        console.log(response);
        fakeRequestCallback(
          "books.com/page3",
          function (response) {
            console.log("IT WORKED AGAIN (3rd req)!!!!");
            console.log(response);
          },
          function (err) {
            console.log("ERROR (3rd req)!!!", err);
          }
        );
      },
      function (err) {
        console.log("ERROR (2nd req)!!!", err);
      }
    );
  },
  function (err) {
    console.log("ERROR!!!", err);
  }
);
```
- Using callbacks can result to a lot of nesting (callback hell).
- The Flow:
  ```js
  asyncOpFunction (url1, 
    successFunction () {
      // for req1;
      asyncOpFunction (url2,  
        successFunction () {
          // for req2;
          asyncOpFunction (url3,
            successFunction () {
              // for req3;
            },
            failFunction () {
              // for req3;
            }
          );
        },
        failFunction () {
          // for req2;
        }
      );
    },
    failFunction () {
      // for req1;
    }
  );
  ```
### Option 2 - Promises
#### Definition
- A `Promise` is a returned **object** representing **the eventual resolution of success or failure of an asynchronous operation** (something that takes time) and its resulting value, to which callbacks are attached, instead of passed in to the function.
  - These callbacks will run depending on whether the promise was `fulfilled` or `rejected`.
  - *Rather than passing callbacks into a function, we wait for the function to return a promise object and attach callbacks to it depending on whether the promise was fulfilled or rejected.*
- Promises allow you to associate handlers with an asynchronous action's eventual success value or failure reason.
  - This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future.
#### 3 States
- **Pending:** initial state where the promise is neither fulfilled nor rejected.
  - A pending promise can either be fulfilled with a value or rejected with a reason (error).
  - When either of these options happens, the associated handlers queued up by a promise's `.then` method are called.
- **Fulfilled:** the async operation was successfully fulfilled.
- **Rejected:** the async operation failed.
#### Promise Methods
- **`.then()`**
  - Attach a callback to run **if the promise is fulfilled**.
  - **Returns a promise, hence can be chained on.**
    - Ex: if this promise was fulfilled, check this promise.
- **`.catch()`**
  - Attach a callback/code to run **if the promise is rejected**.
  - Returns a promise.
#### Basic Building Blocks of Promises
```js
// Define a function that returns a Promise
const makeDogPromise = () => {
  return new Promise((resolve, reject) => {
    // setTimeout is simply to mimick delay of response in real life (5s delay until Promise is resolved or rejected).
    setTimeout(() => {
      const rand = Math.random();
      if (rand < 0.5) {
        // We can resolve Promises with a value, which we can access to it in the callback passed in to .then().
        // Value Ex: what is the data that is responded?
        resolve({ status: 200 });
      } else {
        // We can reject Promises with a value, which we can access to it in the callback passed in to .catch().
        // Value Ex: why was something rejected?
        reject({ status: 404 });
      }
    }, 5000);
  });
};

// Working with a Promise
// Code in .then() runs if Promise is resolved.
// Code in .catch() runs if Promise is rejected.
makeDogPromise()
  .then((res) => {
    console.log("Status Code", res.status);
    console.log("Yay we got a dog!!!");
  })
  .catch((res) => {
    console.log(res.status);
    console.log("No dog..");
  });
```
#### Creating Promises
```js
// THE PROMISE VERSION

const fakeRequestPromise = (url) => {
  // the passed in function always has two parameters that are functions: resolve, reject.
  // If resolve() runs, the Promise is resolved.
  // If reject() runs, the Promise is rejected.
  return new Promise((resolve, reject) => {
    const delay = Math.floor(Math.random() * 4500) + 500;
    setTimeout(() => {
      if (delay > 4000) {
        reject("Connection Timeout :(");
      } else {
        resolve(`Here is your fake data from ${url}`);
      }
    }, delay);
  });
};
```
#### Working with Promises
##### Syntactical Flow
```js
fakeRequestPromise("yelp.com/api/coffee/page1")
  .then(() => {
    console.log("IT WORKED!!!!!! (page1)");
    fakeRequestPromise("yelp.com/api/coffee/page2")
      .then(() => {
        console.log("IT WORKED!!!!!! (page2)");
        fakeRequestPromise("yelp.com/api/coffee/page3")
          .then(() => {
            console.log("IT WORKED!!!!!! (page3)");
          })
          .catch(() => {
            console.log("OH NO, ERROR!!! (page3)");
          });
      })
      .catch(() => {
        console.log("OH NO, ERROR!!! (page2)");
      });
  })
  .catch(() => {
    console.log("OH NO, ERROR!!! (page1)");
  });
```
##### Promise Chaining (cleaner version)
```js
fakeRequestPromise("yelp.com/api/coffee/page1")
  .then((data) => {
    console.log("IT WORKED!!!!!! (page1)");
    console.log(data);
    return fakeRequestPromise("yelp.com/api/coffee/page2");
  })
  .then((data) => {
    console.log("IT WORKED!!!!!! (page2)");
    console.log(data);
    return fakeRequestPromise("yelp.com/api/coffee/page3");
  })
  .then((data) => {
    console.log("IT WORKED!!!!!! (page3)");
    console.log(data);
  })
  .catch((err) => {
    console.log("OH NO, A REQUEST FAILED!!!");
    console.log(err);
  });
```
- **Avoid nesting multiple initial functions by returning a promise from within the callback in order to chain `.then()`**.
- **`data`** in **`.then()`** refers to the response/returned value from the fulfilled `Promise`.
- **`.catch()`** can be reduced to one for all.
### Option 3 - Async/Await Functions
- A function that is declared with the **`async`** keyword, and permits the **`await`** keyword to be used in it.
- *The two keywords enable asynchronous, **promise-based** behavior to be written in a cleaner style (don't have to explicitly configure promise chains).*
#### Keywords
- **`async`**
  - Always return a promise.
    - No need to explicitly indicate to return a Promise.
  - **If the function returns a value, the promise will be fulfilled with that value.**
  - **If the function throws an error, the promise will be rejected.**
  - Example
    ```js
    async function hello() {
      return "Hey guys!";
    }
    hello(); // Promise {<fulfilled>: "Hey guys!"}
    
    async function ohNo() {
      throw new Error("Oh no!");
    }
    ohNo(); // Promise {<rejected>: Error: Oh no!}
    ```
  - Async Arrow Function Example
    ```js
    const hello = async () => {
      return "Hey guys!"
    }
    ```
- **`await`**
  - Used to wait for a `Promise` to be fulfilled.
  - **Pauses the execution of the async function that it is inside of, waiting for a `Promise` to be fulfilled, after which the response/returned value can be extracted from, before continuing on.**
  - Code noted by the `await` keyword is a **blocking operation**.
  - Example
    ```js
    const getJoke = async () => {
      const data = (await fetch("https://v2.jokeapi.dev/joke/Any?safe-mode").json();
      return data;
    };
    
    const main = async () => {
      console.log("before");
      console.log(await getJoke());
      console.log("after");
    };
    
    main();
    console.log("end");
    ```
    ```zsh
    before
    end <-- JS continues executing the script
    // data ... <-- data was fetched from the API
    after <-- this was blocked by the API fetch
    ```
- **`return`**
  - If the function `return`s a value, the `Promise` will be fulfilled with that value.
- **`throw`**
  - If the function `throw`s an exception/error, the promise will be rejected.
- **`try...catch`** Statement
  - Handle rejection in async functions using `try...catch` statement.
  - Execute codes in `try {}`, and when an exception is thrown, execute codes in `catch {}`.
  - When an error is caught, the remaining codes are not executed. So, we could include code in `catch {}` that we want to execute despite error.
#### Note
- Returning a value inside a `setTimeout()` doesn't return a `Promise` that could be `await`ed.
- Therefore, it needs to be wrapped with a `Promise` for it to work as intended.
#### Example
```js
const fakeRequest = (url) => {
  return new Promise((resolve, reject) => {
    const delay = Math.floor(Math.random() * 4500) + 500;
    setTimeout(() => {
      if (delay > 4000) {
        reject("Connection Timeout :(");
      } else {
        resolve(`Here is your fake data from ${url}`);
      }
    }, delay);
  });
};
```
```js
async function makeTwoRequests() {
  try {
    let data1 = await fakeRequest("/page1");
    console.log(data1);
    let data 2 = await fakeRequest("/page2");
    console.log(data2);
  } catch (err) {
    console.log("CAUGHT AN ERROR!");
    console.log("Error is:", err);
  }
}
```
- Created an async function in which the previously created `Promise` will be called.
- "Wait for this request to return. If it is fulfilled/returned, store it in a variable."
#### Sequential vs. Parallel Requests
- `await`ing in sequence or `await`ing in parallel for `async` functions.
##### Sequential Requests
- Each `await` line of code has to finish completely before the next line of code runs.
- They are not being sent off at the same time.
- If the following requests are dependent on the previous (ex: url from previous response).
- Example
  ```js
  async function get3Pokemon() {
    // poke variables are not the Promises.
    // the await keyword waits for a resolved value, which is then stored in the poke variables.
    const poke1 = await axios.get("https://pokeapi.co/api/v2/pokemon/1");
    const poke2 = await axios.get("https://pokeapi.co/api/v2/pokemon/2");
    const poke3 = await axios.get("https://pokeapi.co/api/v2/pokemon/3");
    console.log(poke1.data);
    console.log(poke2.data);
    console.log(poke3.data);
  }

  get3Pokemon();
  ```
##### Parallel Requests
- If the requests are independent endpoints and are not related, we can have the requests go in parallel.
- Send each request roughly at the same time and await each response (but not after one another).
- If we don’t need requests to be resolved in sequence, do them in parallel.
- Example
  ```js
  async function get3Pokemon() {
    // variables are Promises (not the resolved values).
    const prom1 = axios.get("https://pokeapi.co/api/v2/pokemon/1");
    const prom2 = axios.get("https://pokeapi.co/api/v2/pokemon/2");
    const prom3 = axios.get("https://pokeapi.co/api/v2/pokemon/3");
    // variables are resolved values from the Promises.
    const poke1 = await prom1;
    const poke2 = await prom2;
    const poke3 = await prom3;
    console.log(poke1.data);
    console.log(poke2.data);
    console.log(poke3.data);
  }

  get3Pokemon();
  ```
###### `Promise.all()`
> Takes an iterable of promises as an input, and returns a single Promise that resolves to an array of the results of the input promises. This returned promise will resolve when all of the input’s promises have resolved, or if the input iterable contains no promises. It rejects immediately upon any of the input promises rejecting or non-promises throwing an error, and will reject with this first rejection message / error. | MDN
- Example
  ```js
  async function get3Pokemon() {
    // variables are Promises (not the resolved values).
    const prom1 = axios.get("https://pokeapi.co/api/v2/pokemon/1");
    const prom2 = axios.get("https://pokeapi.co/api/v2/pokemon/2");
    const prom3 = axios.get("https://pokeapi.co/api/v2/pokemon/3");
    // variables are resolved values from the Promises.
    const results = await Promise.all([prom1, prom2, prom3]); // responds with an array containing 3 response objects.
    printPokemon(results);
  }

  function printPokemon(results) {
    for (let pokemon of results) {
      console.log(pokemon.data.name);
    }
  }

  get3Pokemon();
  ```

## AJAX and Web APIs
### AJAX/AJAJ
- Asynchronous JavaScript And XML/JSON.
- Refers to making JS requests asynchronously to load/send information from/to the server behind the scenes.
  - Basically, making requests on the page after it has been already been loaded (not in-between page loads).
  - Ex: live search, infinite scrolling, etc.
- *The server responds with XML or JSON file with plain old information/data, instead of a web page of HTML, CSS, JS.*
- Concept: create applications where using JS, we can load, fetch, send, save data to a database behind the scenes without refreshing the page.
### Web API
- In broad terms, API refers to any interface that defines interactions between multiple software applications or mixed hardware-software intermediaries.
- **Web APIS are web-based interfaces that are HTTP-based and make requests to specific endpoints/URL that provide specified data.**
  - Endpoints respond with information for other pieces of software to consume.
  - A "portal" into a different application/database.
- The messenger that take requests and tells a system what you want to do, and then returns the response back to you.
  - Ex: waiter that takes your order to the kitchen, and then deliver your food back to your table.
### XML
- Extensible Markup Language.
- A generic markup language.
- A way of grouping content and adding meaning to the data.
### JSON
- JavaScript Object Notation (JSON).
- A format for sending data.
- Every key must be a string with quotes.
- Methods
  - **`JSON.parse()`**
    - Convert JSON to JS object.
    - When receiving information from the API.
  - **`JSON.stringify()`**
    - Convert JS object to JSON.
    - When sending information to the API.
- Refer [here](https://www.json.org/json-en.html).
### HTTP Request and Response
#### Types of HTTP Requests
- Get
- Post
- More [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
#### HTTP Response
- Body
  - The content/payload of the response that we got back.
  - Depending on what has been requested, can be HTML, JSON, etc.
- Status Codes
  - Numeric codes that are standardized for a quick way of indicating, from the server to the client, how the request went.
  - Numbers starting with 2 indicate success.
  - Numbers starting with 3 indicate redirection.
  - Numbers starting with 4 indicate client error responses.
  - Numbers starting with 5 indicate server error responses.
- Query Strings & Headers
  - Query String
    - Key-Value pairs representing additional information that we pass in to any URL.
    - Ex: ?q=?:query
      - query is a parameter for which the client provides.
  - Headers
    - Key-Value pairs that are like meta data for a request or response.
      - Key is the parameter.
      - Value is the argument.
      - Ex: Content-Type, Last-Modified, etc.
    - Some APIs require certain headers to be specified when sending request. Read the docs!
### Sending HTTP Requests via JS
#### Option 1 - Making XHRs
- XMLHttpRequest(XHR).
- Does not support `Promises` (so, callback hell esp. when making subsequent requests).
- Used to be the only way to make requests.
- Example
  ```js
  // Make a new request object.
  const req = new XMLHttpRequest();

  // Attach an onload callback.
  req.onload = function () {
    console.log("ALL DONE WITH REQUEST!!!");
    // Need to parse JSON file in order to use in JS.
    const data = JSON.parse(this.responseText);
    console.log(data.ticker.price);
  };

  // Attach an onerror callback.
  req.onerror = function () {
    console.log("ERROR!!!");
    console.log(this);
  };

  // Open the request.
  // Specify what type of request, and what url to send it to.
  req.open("GET", "https://api.cryptonator.com/api/ticker/btc-usd");
  // Send the request.
  req.send();
  console.log("Request Sent!"); // This executes before data arrives.
  ```
#### Option 2 - Fetch API
- Syntax: `fetch("url")`.
- **Returns a `Promise` that is fulfilled with a *response object*, which the content of it is included as a readable stream.**
- **Caveat 1:**
  - `fetch()` fulfills the `Promise` as soon as it gets the first bit of headers even if the data (body/content that was requested) has not arrived yet. Hence, the complete data may not be included in the response object that was returned.
- So, use the **`.json()`** method on the response.
  - `.json()` takes the `Response` steram (response object) and reads it to completion and returns a `Promise` object.
- `res.json()` returns a `Promise` object after waiting for the body/content to arrive.
  - The downside is that it could take some time depending on how large the steram is.
- **Caveat 2:**
  > The Promise returned form fetch() won’t reject on HTTP error status even if the response is an HTTP 404 or 500. Instead, it will resolve normally (with ok status set to false), and it will only reject on network failure or if anything prevented the request from completing. | MDN
  - Therefore, `.catch()` won’t run just because the server responded with something that’s not a status `ok` because we still got a response.
    - `.then()` runs whenever a response is received; whether it is the response we wanted or not.
    - `fetch()` will only return a rejected promise if our internet is not working, the request can’t be made at all, or if we just don’t get a response.
- `Promise.resolve()`
  - Returns a Promise object that is resolved with a given value.
  - Useful for refactoring fetch chains.
  - Example
    ```js
    const checkStatusAndParse = (res) => {
      if (!res.ok) throw new Error(`Status Code Error: ${res.status}`);

      return res.json();
    };
    const printPlanets = (data) => {
      console.log("Loaded 10 more planets...");
      for (let planet of data.results) {
        console.log(planet.name);
      }
      // This Promise only exists to keep the chain of .then() going.
      return Promise.resolve(data);
    };

    const fetchNextPlanets = (url) => {
      return fetch(url);
    };

    fetch("https://swapi.co/api/planets/")
      .then(checkStatusAndParse)
      .then(printPlanets)
      .then(fetchNextPlanets)
      .then(checkStatusAndParse)
      .then(printPlanets)
      .catch((err) => {
        console.log("Something went wrong with fetch!");
        console.log(err);
      });
    ```
##### Approach 1 - `.then()` and `.catch()`
```js
// fetch API using .then() and .catch()

fetch("https://api.cryptonator.com/api/ticker/btc-usd")
  .then((res) => {
    if (!res.ok) {
      // this new Error will trigger the callback in .catch() with the error that has been passed in.
      throw new Eror(`Status Code Error: ${res.status}`);
    } else {
      console.log("RESPONSE, WAITING TO PARSE...", res);
      return res.json();
    }
  })
  .then((data) => {
    console.log("DATA PARSED...");
    console.log(data.ticker.price);
  })
  .catch((err) => {
    console.log("Something went wrong with fetch!", err);
  });
```
##### Approach 2 - `async` and `await`
```js
const fetchBitcoinPrice = async () => {
  try {
    const res = await fetch("https://api.cryptonator.com/api/ticker/btc-usd");
    const data = await res.json();
    console.log(data.ticker.price);
  } catch (err) {
    console.log("SOMETHING WENT WRONG!!!", err);
  }
};
```
#### Option 3 - AXIOS
- An external library for making HTTP Requests.
- Built on top of Fetch, with less steps.
  - No need to parse JSON.
  - No need to weed out the bad status codes.
    - Responses with a non `200` status code (Ex: 404) will be rejected.
- Works with both client-side and server-side (Node.js) with the same syntax.
- Method
  - **`axios.get()`**
    - Returns a `Promise` object that already has parsed data in it.
      - No need for `res.json()` and multiple `.then()`s.
      - Unlike `fetch()`, the `Promise` is returned once everything is finished (requested data is included).
##### Approach 1 - `.then()` and `.catch()`
```js
// axios.get() using .then().catch()

axios
  .get("https://api.cryptonator.com/api/ticker/btc-usd")
  .then((res) => {
    console.log(res.data.ticker.price);
  })
  .catch((err) => {
    console.log("ERROR!", err);
  });
```
##### Approach 2 - `async` and `await`
```js
// axios.get() using async await
const fetchBitcoinPrice = async () => {
  try {
    const res = await axios.get(
      "https://api.cryptonator.com/api/ticker/btc-usd"
    );
    console.log(res.data.ticker.price);
  } catch (err) {
    console.log("ERROR!", err);
  }
};
```
##### Configuring Request Headers with AXIOS
- `axios.get()` accepts a second argument after the endpoint url for configuration information.
- Ex: `const config = { headers: { Accept: "applcation/json" }, params: { q: searchInput }}`
  - `params` for query strings, ID, country, embed etc.
###### Example
```js
// Axios Headers Config

const getDadJoke = async () => {
  try {
    const config = { headers: { Accept: "application/json" } };
    const res = await axios.get("https://icanhazdadjoke.com/", config);
    return res.data.joke;
  } catch (e) {
    return "NO JOKES AVAILABLE! SORRY :(";
  }
};
```
##### AXIOS and DOM
###### Example
```js
// AXIOS and DOM Example

const btn = document.querySelector("button");
const jokes = document.querySelector("#jokes");

// Function for API request for returning the joke text.
const getDadJoke = async () => {
  try {
    const config = { headers: { Accept: "application/json" } };
    const res = await axios.get("https://icanhazdadjoke.com/", config);
    return res.data.joke;
  } catch (e) {
    return "NO JOKES AVAILABLE! SORRY :(";
  }
};

// Async function for adding new joke.
const addNewJoke2 = async () => {
  // create an li
  const newJoke = document.createElement("li");
  // generate the random joke
  const randJokeText = await getDadJoke();
  // append
  newJoke.append(randJokeText);
  jokes.append(newJoke);
};

// Adding new joke to the page on button click.
btn.addEventListener("click", addNewJoke2);
```

## Reference
[Asynchronous flow control - Node.js](https://nodejs.dev/en/learn/asynchronous-flow-control/)  
[JavaScript Asynchronous Programming and Callbacks - Node.js](https://nodejs.dev/en/learn/javascript-asynchronous-programming-and-callbacks/)  
[JavaScript Engine and the Event Loop](https://www.epineda.net/javascript-engine-and-the-event-loop/)  
[A Visual Explanation of JavaScript Event Loop](https://www.javascripttutorial.net/javascript-event-loop/)  
[Behind Node.js - The Event Loop](https://nexocode.com/blog/posts/behind-nodejs-event-loop/)  
[The Node.js Event Loop, Timers, and process.nextTick() | Node.js](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)  
[Basics of libuv - libuv documentation](https://docs.libuv.org/en/v1.x/guide/basics.html)  

[JavaScript Event Loop: How it Works and Why it Matters in 5 Minutes - YouTube](https://www.youtube.com/watch?v=lqLSNG_79lI&ab_channel=JamesQQuick)  
[Asynchronous JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)  
[What is an API? (Application Programming Interface) | MuleSoft](https://www.mulesoft.com/resources/api/what-is-an-api#:~:text=API%20is%20the%20acronym%20for,you're%20using%20an%20API.)  
[Using the Fetch API - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)  
[axios/axios: Promise based HTTP client for browser and node.js](https://github.com/axios/axios)  
