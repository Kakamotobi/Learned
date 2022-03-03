# API

## Table of Contents
- [What is an API?](#what-is-an-api)
- [Types of Web APIs](#types-of-web-apis)
- [Reference](#reference)

## What is an Application Programming Interface (API)?
- **A software intermediary (interface), with set rules, that allows two applications to interact with each other.**
  - i.e. a type of software interface, that offers a service to other softwares.
- "Application" refers to a piece of software with a distinct function OR a whole server/app or a small part of an app.
- APIs are used by software applications in much the same way that interfaces for apps and software are used by us (people).
- *API is essentially a waiter that takes the customer's (client) order (request) to the kitchen (system), and then delivers the food (response) back to the customer.*
- When people say "API", they usually are referring to Web API that exposes an application's data and functionality.
### Elaboration
- Every web page visit is an interaction with some remote server’s API.
- An API is not the remote server itself - rather, it is the part of the server that receives requests and sends responses.
  - To render the whole web page, the browser/client expects a response in HTML.
  - For a specific part of an app, the browser/client could expect simply data.
### How an API Call Goes
1. A client application makes an API call (request) to an endpoint of a web server.
2. The API receives the request and relays it to the server.
3. The server sends a response with the requested information to the API.
4. The API delivers the payload back to the client application.

## Types of Web APIs
- Open APIs (a.k.a. Public APIs)
- Partner APIs
- Internal APIs
- Composite APIs
- Browser APIs
  - Browsers come with APIs, which are methods that can be called from JS.
  - The JS call stack recognizes these web API functions and passes them off to the browser to take care of. Once the browser finishes those tasks, they return and are pushed onto the stackas a callback.
  - Ex: `setTimeout()`, web audio API (audio manipulation).

## Reference
[What is an Application Proramming Interface (API) | IBM](https://www.ibm.com/cloud/learn/api)  
[What is an API? (Application Programming Interface) | MuleSoft](https://www.mulesoft.com/resources/api/what-is-an-api)  
