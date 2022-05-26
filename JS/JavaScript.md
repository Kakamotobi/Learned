# JavaScript
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



## Reference
[JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)  
