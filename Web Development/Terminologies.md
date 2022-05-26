# Terminologies

## Table of Contents
- [Static Web Page vs. Dynamic Web Page](#static-web-page-vs-dynamic-web-page)
- [Time to View (TTV) and Time to Interact (TTI)](#time-to-view-ttv-and-time-to-interact-tti)
- [Library vs. Framework](#library-vs-framework)

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
