# JavaScript

## Table of Contents
- [About JavaScript](#about-javascript)
- [JavaScript History](#javascript-history)
- [JavaScript Engine](#javascript-engine)
- [JavaScript Runtime Environment](#javascript-runtime-environment)
  - ["Runtime"](#runtime)
    - [The Process](#the-process)
      - [JS Engine](#js-engine)
        - [Memory Heap](#memory-heap)
          - [Memory Management](#memory-management)
        - [Call Stack](#call-stack)
          - [Execution Context](#execution-context)
      - [Event Loop](#event-loop)
      - [Web APIs](#web-apis)
      - [Queues](#queues)
        - [Callback Queue/Task Queue](#callback-queuetask-queue)
        - [Microtask Queue](#microtask-queue)
  - [How JavaScript is single-threaded, yet, is non-blocking, asynchronous, and concurrent](#how-javascript-is-single-threaded-yet-is-non-blocking-asynchronous-and-concurrent)
  - [Optimization Insights](#optimization-insights)
- [Node.js Worker Thread Module](#nodejs-worker-thread-module)
  - [Structure](#structure)
  - [How Workers Run in Parallel](#how-workers-run-in-parallel)
  - [Example](#example)

## About JavaScript
> JavaScript (JS) is a lightweight, interpreted, or just-in-time compiled programming language with first-class functions. | MDN
- **[JIT compiled](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#just-in-timejit-compiler)**

> JavaScript is a prototype-based, multi-paradigm, single-threaded, dynamic language, supporting object-oriented, imperative, and declarative (e.g. functional programming) styles. | MDN
- **[Prototype-based](https://github.com/Kakamotobi/Learned/blob/main/JS/Prototypes-Classes-OOP/Prototypes-Classes-OOP.md)**
- **Single-threaded**
  - There is only one thread (worker) available. Therefore, at any given point in time, JavaScript runs one line of code.
    - i.e. only one thing happens at a time.
  - *Since JS is single-threaded, it is synchronous in nature.*
- **Synchronous**
  - Tasks are executed sequentially (start and complete execution one by one).
    - Ex: if one task takes 5s to complete execution, the following code will have to wait their turn for 5s.
  - Nothing happens until the response gets back (blocking, a.k.a. beach ball).
  - **[Asynchronous JavaScript](https://github.com/Kakamotobi/Learned/blob/main/JS/Asynchronous/Async.md)**
    - *JavaScript is only asynchronous in that it can hand off certain tasks to the browser (AJAX, etc.) to handle while it synchronously runs through the script.*
    - JavaScript runtime environments (browsers, node.js) allow you to run background jobs via web workers (worker thread). This is achieved through the Event Loop.
- **[Dynamic language](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#dynamic-typing)**

## JavaScript History
- The Netscape Navigator (web browser founded in 1994)'s founders realized that browsers needed to become more dynamic and web designers needed some sort of language to make websites more interactive. Hence, they turned to the general programming language Java, which they realized it was too complicated for web designers and non-programmers to use. Therefore, in 1995, Brendan Eich was hired to put the Scheme programming language in the browser but maintain a syntax resembling Java. 10 days later, the first version of JavaScript was born, named Mocha.
  - Syntactically, Mocha resembled other curly bracket languages like Java or C, but it included features such as first-class functions (like Scheme), dynamic typing (like Lisp), and prototypes (like Self).
  - Mocha was not a specialized language only for browsers in the 90s. Instead, it was a flexible, multi-paradigm language that developers could use to apply their own language patterns too.
- In September 1995, Mocha was renamed to LiveScript and was shipped in the first beta releases of Netscape Navigator 2.0.
- In December 1995, LiveScript was renamed to JavaScript in a marketing attempt.
- JavaScript started impacting user experience mostly in the form of pop-up windows.
- In 1996, Microsoft, launching Internet Explorer, reverse engineered JavaScript and named it JScript.
- In 1997, there was a need to standardize JavaScript, which led to the born of ECMAScript.
  - ECMAScript provided browser vendors and server-side applications a consistent set of guidelines for implementing the JavaScript language.
- In December 1999, ECMAScript version 3 was released.
  - Fixing problems such as lenient equality testing, and better error handling.
- Netscape was acquired by AOL, Microsoft's Internet Explorer took over the browser market share and did not care for complying to ECMAScript's spec, implementing its own extensions for JavaScript.
  - This led to some revolutionary features such as AJAX, which is a precursor to modern day SPAs.
- In the early 2000s ES3.1 and ES4 (major changes to the language) proposals competed, which saw ES4 abandoned ultimately in 2008.
  - However, in 2006, Adobe created a scripting language supported by Flash called ActionScript, which complied to ES4.
- In the mid 2000s, developers struggled to create web applications that ran on all browsers.
- In 2006, jQuery was born, which allowed developers to create applications that work more reliably on all browsers.
- In September 2008, Google Chrome and the V8 Engine was born.
  -V8 completely changed how JavaScript was compiled and interpreted, enabling applications requiring higher performance both in the browser and server-side. 
- In May 2009, Node.js was born, becoming a great solution for building real-time web applications that scale.
  - Node.js meant that developers could build a whole web application using a single programming langauge.
- In December 2009, 10 years after its previous official spec, ECMAScript released ES5, which used ES3.1 as the basis.
  - ES5 included JSON support, functional array and object methods, strict mode, accessors, etc.
- In 2010, JavaScript frameworks designed specifically for SPAs began to roll out.
  - In October 2010, Backbone.js and Angular.js, each with its own approach, were released to solve the same problem.
    - Imperative vs declarative style, respectively.
    - The founder of Backbone.js also created CoffeeScript, which enabled transpiling to go mainstream.
- In 2015, ES6 (later renamed to ES2015) was released.
  - ES2015 included promises, `let` and `const`, arrow functions, spread, destructuring, etc.
  - However, it was hard for developers to use these features because many legacy browsers did not support them.
  - Hence, developers resorted to transpilers such as Babel and TypeScript because they can convert the code to previous ES versions supported by legacy browsers.
- In 2015, React.js was born.
  - React adopted declarative UI (like Angular.js) but improved it with a unidirectional data flow, immutability and the use of the virtual DOM.
- Around this period, bundlers such as Webpack and Rollup were introduced to manage the complexities of these heavyweight JavaScript applications.
- Henceforth, ECMAScript is making proposals and adjustments to JavaScript on a yearly basis.

## JavaScript Engine
- **A program that reads, interprets, compiles the source code and executes the corresponding machine code.**
- Used in JavaScript runtime environments like web browsers and Node.js.
- Ex: Google's V8
### The Process
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/JS/refImg/js-engine.png" alt="JS Engine/Interpreter" width="80%" />
</p>

1. **Parser**
    - While parsing through the HTML, JS script tags are encountered. 
    - The source code in these scripts are loaded to a byte stream decoder as a UTF-16 byte stream.
    - The byte stream decoder decodes the bytes into token and sends to the parser.
2. **Abstract Syntax Tree (AST)**
    - The parser creates nodes based on the tokens it receives.
    - These nodes are used to create an AST.
3. **Interpreter**
    - The interpreter walks through the AST and generates byte code, reading the code line by line.
    - When the byte code is generated, the AST is deleted, clearing up memory space.
    - Note: interpreters running the same code multiple times can get slow, so compiler is used for those code.
4. **Profiler**
    - Monitors and watches code to optimize it.
5. **Compiler**
    - The compiler works ahead of time and creates a translation of the source code into machine language.
    - Ex: Babel (modern JS to browser compatible JS), Typescript (transcompiles to JS).

## JavaScript Runtime Environment
- **The location/environment where your program will be executed in.**
  - Ex: web browsers' (Ex: chrome, firefox, safari) runtime environment, Node.js.
- It uses a JavaScript engine and provides APIs for some functionalities.
  - Browser APIs Ex: DOM manipulation APIs, window and document APIs.
  - Node.js APIs Ex: APIs for server application (require, process, buffer APIs).
### "Runtime"
- Refers to the period when your program is executing commands (after compilation, if compiled).
- **Runtime error** refers to an error that occurs while a program is running.
  - Distingiushed from *syntax* errors and *compilation* errors, which occur before a program is run.
- When a program is in runtime, the application is loaded into memory (RAM). When the program is done, the runtime period ends and the memory that was being used by the program is made available again.
### The Process
1) The JS engine begins executing the script line by line.
2) Declared variables and objects are stored in the **Memory Heap**.
3) Function calls are added to the **Call Stack**.
    - Any callbacks are added to the **Call Stack**.
      - If the stack size exceeds what it had available, this leads to a "stack overflow" error.
    - If the function is an asynchronous task, it is passed on over to the **Web APIs** to deal with.
    - When the current function is done executing, it is removed from the **Call Stack**. The JS engine resumes executing the script from where it left off. 
4) Once the asynchronous task is done, it is pushed onto either the **Microtask Queue** or the **Callback Queue**.
5) When the **Call Stack** is empty, the **Event Loop** takes the finished task waiting in the **Microtask Queue** or **Callback Queue** back onto the **Call Stack** to be executed.

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/JS/refImg/js-runtime-environment.png" alt="JS Runtime Environment" width="80%" />
</p>

#### JS Engine
##### Memory Heap
- **The place where variables, objects, etc. used by the JavaScript program are stored in.**
  - Ex: when a variable is declared, a location in the memory heap is allocated to the variable.
- It is the free space inside your OS.
  - This memory is limited.
###### Memory Management
- **Memory Leak**
  - Memory leak occurs if all references to the variable is lost and there is no way to access it anymore.
  - JavaScript provides **garbage collection**, which detects and reclaims space allocated to variables that are out of context and will not be used further.
    - V8's garbage collector uses an algorithm called Mark and Sweep.
    - *However, developers still need to care for memory management since garbage collection is just an approximation and it is difficult to exactly determine whether or not the allocated memory is no longer needed.*
  - Some common causes of memory leak:
    - Global variables
      - They will take up space throught the execution of the program even if they are not needed.
    - Event listeners
      - If they are not removed when the user "goes" to another page in an SPA, they keep adding up and taking memory.
    - Removed DOM elements
      - Even if the DOM element is removed, the memory is not reclaimed because it is still being referenced to in the event listener.
##### Call Stack
- a.k.a. **Execution Stack**.
- **The stack data structure where _execution contexts_ of functions (and global) are placed when the functions are called.**
  - It is the mechanism that the JS engine uses to keep track of its place in a script and what's being executed.
- Functions (execution contexts) are popped off from the call stack when they return or if they are API calls, which are delegated to the **Web API container** (browser).
- Each thread has its own call stack.
  - Therefore, single-threaded languages have one call stack.
  - Whereas, multi-threaded languages have multiple call stacks.
###### Execution Context
> ...an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context. | Bits and Pieces

- An Execution Context contains information such as the function's arguments, local variables and its lexical environment (closure).
  - Closures are created using information from the execution context of the outer function.
- When a function finishes executing, its execution context is popped off from the call stack.
- **Types of Execution Context**
  - **Global Execution Context**
    - The base execution context when the JS Engine starts going through a JS script.
    - _Everything that is not nested inside any block (Ex: function) resides in the global execution context._
    - The global execution context is popped off from the call stack when the JS engine reaches the end of the JS file.
  - **Functional Execution Context**
    - Whenever a function invocation is encountered, the JS Engine creates an execution context for that function and pushes it to the call stack.
      - i.e. an execution context is created at the time of function invocation. Therefore, 
  - **Eval Execution Context**
    - The script executed inside an `eval()` function has its own execution context.
- **Phases of an Execution Context**
  - **Creation Phase**
    - The `LexicalEnvironment` component is created.
    - The `VariableEnvironment` component is created.
  - **Execution Phase**
#### [Event Loop](https://github.com/Kakamotobi/Learned/blob/main/JS/Asynchronous/Async.md#the-event-loop)
- **Responsible for executing the code, collecting and processing events, and executing queued sub-tasks.**
- A mechanism on the browser, not the JavaScript engine.
- It's job is to manage the call stack and callback queue.
  - If the call stack is empty, the event loop takes the first thing in the callback queue and pushes it onto the stack.
- This is what essentially allows a single-threaded language like JavaScript to be able to execute tasks asynchronously.
#### Web APIs
- **These are part of the JS engine (i.e. V8 does not take care of these).**
- These are extra things that the browser takes care of.
- Ex: DOM, AJAX requests, Events, `setTimeout()`.
#### Queues
- The Event Loop uses two queues to handle different tasks: **Callbacks** and **Microtasks**.
  - **These queues are where asynchronous code, when done, are pushed to, waiting to be pushed back to the call stack.**
  - The **Microtask Queue** has a higher priority than the **Callback Queue**.
    - Therefore, all tasks on the **Microtask Queue** is first taken care of before tasks on the **Callback Queue**.
    - Example
      ```js
      console.log("I was executed in the Call Stack");
      
      setTimeout(() => console.log("Callback Queue 1"), 0);
      setTimeout(() => console.log("Callback Queue 2"), 0);
      
      Promise.resolve().then(() => console.log("Microtask Queue 1"));
      Promise.resolve().then(() => console.log("Microtask Queue 2"));
      ```
      ```js
      // I was executed in the Call Stack
      // Microtask Queue 1
      // Microtask Queue 2
      // Callback Queue 1
      // Callback Queue 2
      ```
- Both queues contain callbacks.
##### Callback Queue/Task Queue
- **Tasks that are placed in the Callback Queue include callbacks from timers (`setTimeout()`, `setInterval()`), `requestAnimationFrame()`, UI rendering, etc.**
- Ex: the callback function in `setTimeout()` is placed in the Callback Queue.
##### Microtask Queue
- **Tasks that are placed in the Microtask Queue include callbacks from Promises, MutationObserver.**
- Ex: the callback function in `fetch()` is placed in the Microtask Queue.
### How JavaScript is single-threaded, yet, is non-blocking, asynchronous, and concurrent
- The reason JavaScript is able to do things concurrently is because the browser is more than just the runtime.
- The Web APIs take care of asynchronous tasks (Ex: API call). While the asynchronous task is running, code can run on the **Call Stack** (synchronously). Hence, JS can be *non-blocking*.
- If the **Call Stack** is empty, the **Event Loop** takes the first task waiting in the **Microtask Queue** or, if none, the first task in the **Callback Queue** and pushes it to the **Call Stack**.
### Optimization Insights
- The browser would like to repaint the screen asap. But it is constrained by the script.
  - It cannot render if there is code on the **Call Stack**. It has to wait until the stack is clear.
  - Rendering has a higher priority than **Callback Queue**.
    - The browser will render inbetween tasks in the **Callback Queue**.
  - When the render is blocked, users cannot interact with the screen.
- Having slow code on the **Call Stack** ("blocking the event loop") or long callbacks can block code.
  - The browser will not be able to do what it needs to (Ex: create a nice fluid UI).
  - Callbacks should be kept relatively short and simple.
- Do not flood the **Callback Queue**.
  - Ex: scroll animations don't take up the call stack a lot but can flood the callback queue.
  - Possible solution: debouncing.

## Node.js Worker Thread Module
- Worker Threads exist in Node.js.
- cf. Web Workers exist in browsers.

<p align="center">
  <img src="https://images.ctfassets.net/hspc7zpa5cvq/20h5efXHT4bQbuf44mdq2H/a40944191d031217a9169b17a8ef35d6/worker-diagram_2x__1_.jpg" alt="Node.js Worker Threads" width="80%"/>
</p>

- **This module allow us to use multiple threads to execute JS in parallel.**
  - The main thread (the application's event loop) can stay undisrupted while other threads execute other tasks.
  - Each thread has a dedicated event loop.
- Prior to this module, in order to involve additional CPUs or keep CPU intensive code from blocking the event loop, we had to use multiple processes.
### Structure
<p align="center">
  <img src="https://www.simplilearn.com/ice9/free_resources_article_thumb/Node.jsWorker.png" alt="Parent/Child Worker" width="80%"/>
</p>

- The main thread or parent thread delegates code to be executed to a worker thread.
  - A parent/child workers can communicate through a message channel.
  - A worker is independent of other workers.
### How Workers Run in Parallel
- _V8 allows the create of isolated V8 runtime instances, where each instance has a dedicated heap memory and micro-task queue._
  - i.e. each worker is essentiailly an instance of Node.js.
  - This allows each worker to execute its JS code completely separate from other workers. However, this is also why workers cannot quickly access each other's heap memory.
- When there are worker threads, it means that there are multiple Node instances that are running in the same process in a Node application.
### Example
- **Setup**

  ```js
  import WT from "node:worker_threads";

  const worker = new WT.Worker(
    `
    const { parentPort } = require('worker_threads');
    parentPort.once('message',
        message => parentPort.postMessage({ pong: message }));
    `,
    { eval: true }
  );
  ```

  ```
  { pong: 'ping' }
  ```
- **The Module**
  ```js
  console.log(WT);
  ```

  ```
  {
    isMainThread: true,
    MessagePort: [Function: MessagePort],
    MessageChannel: [Function: MessageChannel],
    markAsUntransferable: [Function: markAsUntransferable],
    moveMessagePortToContext: [Function: moveMessagePortToContext],
    receiveMessageOnPort: [Function: receiveMessageOnPort],
    resourceLimits: {},
    threadId: 0,
    SHARE_ENV: Symbol(nodejs.worker_threads.SHARE_ENV),
    Worker: [class Worker extends EventEmitter],
    parentPort: null,
    workerData: null,
    BroadcastChannel: [class BroadcastChannel extends EventTarget],
    setEnvironmentData: [Function: setEnvironmentData],
    getEnvironmentData: [Function: getEnvironmentData]
  }
  ```
- **A Worker Thread**
  ```js
  console.log(worker);
  ```

  ```
  Worker {
    _events: [Object: null prototype] {
      newListener: [Function (anonymous)],
      removeListener: [Function (anonymous)]
    },
    _eventsCount: 2,
    _maxListeners: undefined,
    performance: { eventLoopUtilization: [Function: bound eventLoopUtilization] },
    [Symbol(kCapture)]: false,
    [Symbol(kHandle)]: Worker {
      messagePort: MessagePort [EventTarget] {
        active: true,
        refed: false,
        [Symbol(kEvents)]: [SafeMap [Map]],
        [Symbol(events.maxEventTargetListeners)]: 10,
        [Symbol(events.maxEventTargetListenersWarned)]: false,
        [Symbol(kNewListener)]: [Function (anonymous)],
        [Symbol(kRemoveListener)]: [Function (anonymous)],
        [Symbol(nodejs.internal.kCurrentlyReceivingPorts)]: undefined,
        [Symbol(kWaitingStreams)]: 0
      },
      threadId: 1,
      onexit: [Function (anonymous)]
    },
    [Symbol(kPort)]: MessagePort [EventTarget] {
      active: true,
      refed: false,
      [Symbol(kEvents)]: SafeMap(3) [Map] {
        'newListener' => [Object],
        'removeListener' => [Object],
        'message' => [Object]
      },
      [Symbol(events.maxEventTargetListeners)]: 10,
      [Symbol(events.maxEventTargetListenersWarned)]: false,
      [Symbol(kNewListener)]: [Function (anonymous)],
      [Symbol(kRemoveListener)]: [Function (anonymous)],
      [Symbol(nodejs.internal.kCurrentlyReceivingPorts)]: undefined,
      [Symbol(kWaitingStreams)]: 0
    },
    [Symbol(kParentSideStdio)]: {
      stdin: null,
      stdout: ReadableWorkerStdio {
        _readableState: [ReadableState],
        _events: [Object: null prototype],
        _eventsCount: 2,
        _maxListeners: undefined,
        [Symbol(kCapture)]: false,
        [Symbol(kPort)]: [MessagePort [EventTarget]],
        [Symbol(kName)]: 'stdout',
        [Symbol(kIncrementsPortRef)]: false,
        [Symbol(kStartedReading)]: false
      },
      stderr: ReadableWorkerStdio {
        _readableState: [ReadableState],
        _events: [Object: null prototype],
        _eventsCount: 2,
        _maxListeners: undefined,
        [Symbol(kCapture)]: false,
        [Symbol(kPort)]: [MessagePort [EventTarget]],
        [Symbol(kName)]: 'stderr',
        [Symbol(kIncrementsPortRef)]: false,
        [Symbol(kStartedReading)]: false
      }
    },
    [Symbol(kPublicPort)]: MessagePort [EventTarget] {
      active: true,
      refed: false,
      [Symbol(kEvents)]: SafeMap(4) [Map] {
        'newListener' => [Object],
        'removeListener' => [Object],
        'message' => [Object],
        'messageerror' => [Object]
      },
      [Symbol(events.maxEventTargetListeners)]: 10,
      [Symbol(events.maxEventTargetListenersWarned)]: false,
      [Symbol(kNewListener)]: [Function (anonymous)],
      [Symbol(kRemoveListener)]: [Function (anonymous)],
      [Symbol(nodejs.internal.kCurrentlyReceivingPorts)]: undefined
    },
    [Symbol(kNewListener)]: [Function (anonymous)],
    [Symbol(kRemoveListener)]: [Function (anonymous)],
    [Symbol(kLoopStartTime)]: -1,
    [Symbol(kIsOnline)]: false
  }
  ```

## Reference
[JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)  
[Introduction to JavaScript Runtime Environments](https://www.codecademy.com/articles/introduction-to-javascript-runtime-environments)  
[Runtime Definition](https://techterms.com/definition/runtime)  
[Why JavaScript is a single-thread language that can be non-blocking ? - GeeksforGeeks](https://www.geeksforgeeks.org/why-javascript-is-a-single-thread-language-that-can-be-non-blocking/)  
[A brief explanation of the Javascript Engine and Runtime | Medium](https://medium.com/@sanderdebr/a-brief-explanation-of-the-javascript-engine-and-runtime-a0c27cb1a397)  
[Uncover the JavaScript: Engine vs Runtime](https://medium.com/@misbahulalam/uncover-the-javascript-engine-vs-runtime-6556ef449634)  
[How Does JavaScript Really Work? (Part 2) | by Priyesh Patel | Bits and Pieces](https://blog.bitsrc.io/how-does-javascript-work-part-2-40cc15360bc)  
[Memory Management - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)  
[Call stack - MDN Web Docs Glossary](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)  
[Understanding Execution Context and Execution Stack in Javascript | by Sukhjinder Arora | Bits and Pieces](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)  
[Understanding JavaScript Execution Context By Examples](https://www.javascripttutorial.net/javascript-execution-context/)  
[A Complete Guide for JavaScript Execution Context](https://www.atatus.com/blog/javascript-execution-context/)  
[The event loop - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)  
[What is the difference between Microtask Queue and Callback Queue in asynchronous JavaScript ? - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-the-difference-between-microtask-queue-and-callback-queue-in-asynchronous-javascript/)  
[Worker threads | Node.js v19.6.0 Documentation](https://nodejs.org/api/worker_threads.html)  
[javascript - What's the difference between Web Workers and Worker Threads? - Stack Overflow](https://stackoverflow.com/questions/60268604/whats-the-difference-between-web-workers-and-worker-threads)  
[Learn all about Nodejs Worker Threads with Examples | Simplilearn](https://www.simplilearn.com/tutorials/nodejs-tutorial/nodejs-worker-threads)  
[Understanding Worker Threads in Node.js - NodeSource](https://nodesource.com/blog/worker-threads-nodejs/)  
