# JavaScript

## Table of Contents
- [About JavaScript](#about-javascript)
- [JavaScript Engine](#javascript-engine)
- [JavaScript Runtime Environment](#javascript-runtime-environment)
  - [Runtime](#runtime)
  - [How JavaScript is single-threaded, yet, is non-blocking, asynchronous, and concurrent](how-javascript-is-single-threaded-yet-is-non-blocking-asynchronous-and-concurrent)
  - [Optimization Insights](#optimization-insights)

## About JavaScript
> JavaScript (JS) is a lightweight, interpreted, or just-in-time compiled programming language with first-class functions. | MDN
- **[JIT compiled](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#just-in-timejit-compiler)**

> JavaScript is a prototype-based, multi-paradigm, single-threaded, dynamic language, supporting object-oriented, imperative, and declarative (e.g. functional programming) styles. | MDN
- **[Prototype-based](https://github.com/Kakamotobi/Learned/blob/main/JS/Prototypes-Classes-OOP/Prototypes-Classes-OOP.md)**
- **Single-threaded**
  - At any given point in time, JavaScript runs one line of code.
  - i.e. only one thing happens at a time.
- **Synchronous**
  - Tasks are executed sequentially (one by one).
  - i.e., if one task takes 5s, the following code will have to wait their turn for 5s.
  - Nothing happens until the response gets back (blocking, a.k.a. beach ball).
  - **[Asynchronous JavaScript](https://github.com/Kakamotobi/Learned/blob/main/JS/Asynchronous/Async.md)**
    - *JavaScript is only asynchronous in that it can hand off certain tasks to the browser (AJAX, etc.) to handle while it synchronously runs through the script.*
    - JavaScript runtime environments (browsers, node.js) allow you to run background jobs via web workers (worker thread). This is achieved through the Event Loop.
- **[Dynamic language](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#dynamic-typing)**

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
- **The stack data structure where function execution contexts are placed when the function is called.**
  - It is the mechanism that the JS engine uses to keep track of its place in a script and what's being executed.
- Functions are popped off from the call stack when they return or if they are API calls, which are delegated to the **Web API container** (browser).
- Each thread has its own call stack.
  - Therefore, single-threaded languages have one call stack.
  - Whereas, multi-threaded languages have multiple call stacks.
#### Event Loop
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
[The event loop - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)  
[What is the difference between Microtask Queue and Callback Queue in asynchronous JavaScript ? - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-the-difference-between-microtask-queue-and-callback-queue-in-asynchronous-javascript/)  
