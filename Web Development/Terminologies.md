# Terminologies

## Table of Contents
- [Static Web Page vs. Dynamic Web Page](#static-web-page-vs-dynamic-web-page)
- [Time to View (TTV) and Time to Interact (TTI)](#time-to-view-ttv-and-time-to-interact-tti)
- [Library vs. Framework](#library-vs-framework)
- [JavaScript Engine vs. JavaScript Runtime Environment](#javascript-engine-vs-javascript-runtime-environment)

## Static Web Page vs. Dynamic Web Page
### Static Web Page
- Web pages that are fixed and display the same content for every user (ex: HTML and CSS).
- Static websites usually come with a fixed number of web pages that have a specific layout.
- The page's source code won't change.
- Informational.
### Dynamic Web Page
- Web page that displays different content and provide user interaction (use advanced programming and databases in addition to HTML).
- *Content of the web page is generated and displayed based on what actions the users make on the page.*
- When a user accesses a dynamic website, the site can be changed through code that is run in the browser and/or on the server.
  - The end result is the same as that on a static website: an HTML page displayed on the browser.
- Combination of client-side and server-side scripting.
#### Client-side Scripting
- Code that is executed by the browser (JS).
#### Server-side Scripting
- Code that is executed by the server (before the content is sent to the user's browser).
- Makes the HTML and CSS functional.
### Reference
[Static vs Dynamic Website: What Is the Difference?](https://wpamelia.com/static-vs-dynamic-website/#:~:text=Static%20websites%20are%20ones%20that,databases%20in%20addition%20to%20HTML.)

---

## Time to View (TTV) and Time to Interact (TTI)
### TTV
- The time it takes for the users to be able to see the contents of a page.
### TTI
- The time it takes for the users to be able to interact with the contents of a page.
- i.e., how long before the page repsonds to a click.
### Reference
[TTV (Time to View) & TTI (Time To Interact)](https://velog.io/@yu2jeong/TTV-Time-To-View-TTI-Time-To-Interact)

---

## Library vs. Framework
### Library
- When using a library, we are in charge. We control the flow of the application code and decide when to use the library.
- Libraries often do smaller things or are single-purpose.
- Ex: React, AXIOS (making http requests), color libraries, ASCII art libraries.
### Framework
- When using a framework, the framework is in charge. The framework provides the structure for an application and we incorporate our code according to how the framework works.
- Trade-off between control, and speed of development and features.
- Frameworks are geared into helping us make full applications.
- Ex: Vue, Angular, Express, Ruby on Rails.
### React
- React is a library, not a framework.
- React is concerned only with rendering the UI and leaves many things up to each project to put together.
- However, upon using other react libraries (Ex: react-router, redux, etc.), the system may resemble a framework.
  - Standard React stack: React, Redux, react-router, webpack, ESLint, etc.

---

## JavaScript Engine vs. JavaScript Runtime Environment
### JavaScript Engine
- **A program that reads, interprets, compiles the source code and executes the corresponding machine code.**
- Used in JavaScript runtime environments like web browsers and Node.js.
- Ex: Google's V8
#### Process
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
### JavaScript Runtime Environment
- **The location/environment where your program will be executed in.**
  - Ex: web browsers'(ex: chrome, firefox, safari) runtime environment, Node.js.
- It uses a JavaScript engine and provide APIs for some functionalities.
  - Browser APIs Ex: DOM manipulation APIs, window and document APIs.
  - Node.js APIs Ex: APIs for server application (require, process, buffer APIs).
#### Runtime
- Refers to the period when your program is executing commands (after compilation, if compiled).
- **Runtime error** refers to an error that occurs while a program is running.
  - Distingiushed from *syntax* errors and *compilation* errors, which occur before a program is run.
- When a program is in runtime, the application is loaded into memory (RAM). When the program is done, the runtime period ends and the memory that was being used by the program is made available again.

### Reference
[A brief explanation of the Javascript Engine and Runtime | Medium](https://medium.com/@sanderdebr/a-brief-explanation-of-the-javascript-engine-and-runtime-a0c27cb1a397)  
[Uncover the JavaScript: Engine vs Runtime](https://medium.com/@misbahulalam/uncover-the-javascript-engine-vs-runtime-6556ef449634)  
[Introduction to JavaScript Runtime Environments](https://www.codecademy.com/articles/introduction-to-javascript-runtime-environments)
[Runtime Definition](https://techterms.com/definition/runtime)

---
