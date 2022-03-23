# Terminologies

## Table of Contents
- [Static Web Page vs. Dynamic Web Page](#static-web-page-vs-dynamic-web-page)
- [Single Page Application (SPA) vs. Multi Page Application (MPA)](#single-page-application-spa-vs-multi-page-application-mpa)
- [Client-Side Rendering (CSR) vs. Server-Side Rendering (SSR) vs. Static-Site Generator (SSG)](#client-side-rendering-csr-vs-server-side-rendering-ssr-vs-static-site-generator-ssg)
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

## Single Page Application (SPA) vs. Multi Page Application (MPA)
### SPA
> A web app implementation that loads only a single web document, and then updates the body content of that single document via JS APIs such as `XMLHttpRequest` and `Fetch` when different content is to be shown.
> Allows users to use websites without loading whole new pages from the server, which can result in performance gains and a more dynamix experience, with some tradeoff disadvantages such as SEO, more effort required to maintain state, implement navigation, and do meaningful performance monitoring.
- SPA frameworks: React, Angular, Vue.JS.
- Ex: Google Maps
#### Pros
- SPA is fast because most resources (HTML, CSS, Scripts) are only loaded once, and only data is transitted back and forth.
- No need to write code to render pages on the server. Easier to get started on a project without having to use any server.
- Feasible debugging, as network operations, page elements, and associated data can be monitored and investigated on browser.
- Can cache any local storage effectively. An application sends only one request, store all the data, then it can use this data and works even offline.
#### Cons
- SEO optimization.
- Slow to download because heavy client frameworks are required to be loaded to the client.
- JS needs to be present and enabled in client's browser.
- Less secure due to Cross-Site Scripting (XSS), which enables attackers to inject client-side scripts into the web app by other users.
- Memory leak in JS can cause system to slow down.
### MPA
- "Traditional" way.
- Every change requests rendering a new page from the server in the browser.
- Larger than SPAs.
#### Pros
- Good for users who need a visual map to navigate the application.
- Good and easier SEO management. It gives better chances to rank for different keywords since an application can be optimized for one keyword per page.
#### Cons
- Frontend and backend development are tightly coupled.
- Development becomes quite complex. Need to use frameworks for either client and server side, requiring longer development time.
### Hybrid Site
- SPA that uses URL anchors as synthetic pages enabling more in browser navigation and preference functionality.
### Reference
[SPA (Single-page application) | MDN](https://developer.mozilla.org/en-US/docs/Glossary/SPA)  

---

## Client-Side Rendering (CSR) vs. Server-Side Rendering (SSR) vs. Static-Site Generator (SSG)
### CSR
> Allows the website to be updated in the browser almost instantly when navigating to different pages, but requires more of an initial download hit and extra rendering on the client at the beginning. The website is slower on an initial visit, but can be faster to navigate.
- Website is rendered on the client-side.
- *When the client sends a request to the server, the server will send a single HTML index page, which will be the skeleton of the page to the client, followed by the JS file. The JS file, which is large as it contains the logic and the source codes of the framework/library, will turn the page into a fully rendered page. If additional content/data is needed, the client makes a request to the API, which then renders the data (JSON) onto the page. If the client navigates to a different route, instead of the server sending the page again, the client will re-render the page according to the route that the client requested. Hence, the page that is used is always the same page as the first request.*
#### Illustration
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/CSR.png" alt="Client-Side Rendering" width="80%" />
</p>

1. Client requests page.
2. Server sends an almost empty `index.html` file. // Users see a blank screen.
3. Client requests the JS files linked in the `index.html` file.
4. JS file containing web application logic (libraries, frameworks) that creates dynamic HTML is received from the server. // TTV, TTI.
    - **Users can now see and interact with the site.**
#### Use If:
- Website has many user interactions.
- Building a web application.
#### Cons
- Low SEO.
  - Web crawlers check the index.html file for titles, etc. but HTML files for CSR are mostly empty, making it difficult for SEO.
- Slow initial loading
#### Question to Think About
- How to effectively divide and prioritize essential content to be sent first in order to quicken TTV and TTI.
### SSR
> Website is rendered on the server, so it offers quicker first load, but navigating between pages requires downloading new HTML content. It works great across browsers, but it suffers in terms of time navigating between pages and therefore general perceived performance - loading a page requires a new round trip to the server.
- Website arrives on the client-side fully-rendered.
- *When the client sends a request to the server, the server compiles everything and renders it (including the requested data) into a fully rendered HTML page, and then send it to the client as a response along with JS source code.
- For every different route that the client navigates to, the server will repeat the steps again.*
#### Illustration
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/SSR.png" alt="Server-Side Rendering" width="80%" />
</p>

1. Client requests page.
2. Server sends an already made `index.html` file. // TTV.
    - **Users can see the site.**
3. Client requests the JS file linked in the html file.
4. JS file is received from the server. // TTI.
    - **Users can now interact with the site.**
#### Use If:
- SEO is priority.
- Need faster initial loading.
- Website doesn't have many user interactions.
#### Cons
- Blinking issue
  - Every click results to bringing the website from the server.
- Server-side overhead.
- Users may need to wait before interacting.
  - Sometimes result to delayed response when clicking on links, etc.
#### Question to Think About
- How to reduce the time between TTV and TTI.
### SSG
> A static site generator is a tool that generates a full static HTML website based on raw data and a set of templates. Essentially, a static site generator automates the task of coding individual HTML pages and gets those pages ready to serve to users ahead of time. Because these HTML pages are pre-built, they can load very quickly in users' browsers.
### Reference
[Progressive web app structure | MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)  
[서버사이드 렌더링](https://www.youtube.com/watch?v=iZ9csAfU5Os&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)  
[What is a static site generator?](https://www.cloudflare.com/learning/performance/static-site-generator/)

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
