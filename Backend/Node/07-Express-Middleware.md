# Express Middleware

## Table of Contents
- [What is it?](#what-is-it)
- [External Middlewares](#external-middlewares)
- [Defining Our Own Middleware](#defining-our-own-middleware)
  - [`next`](#next)
  - [Example 1](#example-1)
  - [Example 2](#example-2)
- [`app.use()`](#appuse)
- [Multiple Callbacks in Route Handlers](#multiple-callbacks-in-route-handlers)
- [Reference](#reference)

## What is it?
> Middleware is a (loosely defined) term for any software or service that enables the parts of a system to communicate and manage data. ...
> In server-side web application frameworks, the term is often more specifically used to refer to pre-built software components that can be added to the framework's request/response processing pipeline, to handle tasks such as database access. | MDN

> Express is a routing and middleware web framework that has minimal functionality on its own: An Express application is essentially a series of middleware function calls. | Express.js

> ***Middleware*** functions are functions that have access to the request object (`req`), the response object(`res`), and the next middleware function in the application's request-response cycle. The next middleware function is commonly denoted by a variable named `next`. | Express.js

- Express middleware are functions that run during the request/response lifecycle.
  - Request --> Middleware --> Response
  - Each middleware has access to the request and response objects.
  - Middleware can end the HTTP request by sending back a response with methods like `res.send()`.
  - OR middleware can be chained together, one after another by calling `next()`.

## External Middlewares

### Morgan - Logger Middleware
- Helps us log HTTP request information to our terminal.
  - Useful for when debugging.
#### Example
```zsh
npm i morgan
```
```js
const express = require("express");
const morgan = require("morgan");

const app = express();

app.use(morgan("dev"));
```

## Defining Our Own Middleware
### `next`
- Use `next()` to execute the following middleware and move on.
### Example 1
```js
const express = require("express");

const app = express();

app.use((req, res, next) => {
  console.log("This is my first middleware!");
  next();
});

app.use((req, res, next) => {
  console.log("This is my second middleware!");
  next();
});

app.get("/", (req, res) => {
  res.send("Home Page!");
});

app.listen(3000, () => {
  console.log("Listening on Port 3000");
});
```
### Example 2
```js
const express = require("express");
const app = express();

app.use((req, res, next) => {
	req.requestTime = Date.now(); // Now, in every one of our route handlers, we have access to req.requestTime
	console.log(req.method, req.path);
	next();
});

app.get("/", (req, res) => {
	console.log(`REQUEST DATE: ${req.requestTime}`);
	res.send("Home Page!");
});

app.get("/dogs", (req, res) => {
	console.log(`REQUEST DATE: ${req.requestTime}`);
	res.send("Woof Woof!");
});

app.listen(3000, () => {
	console.log("Listening on Port 3000");
});
```

## `app.use()`
- Syntax: **`app.use([path,] callback [. callback...])`**
> Mounts the specified middleware function or functions at the specified path: the middleware function is executed when the base of the erquest path matches `path`.
### Example
```js
// Only runs for any incoming request for "/dogs" route/path.
app.use("/dogs", (req, res, next) => {
  console.log("I love dogs!");
  next();
});

// 404 Route
// At the end of the file (if no preceding middlewares/routes have not been triggered).
app.use((req, res) => {
	res.status(404).send("Not Found!");
});
```

## Multiple Callbacks in Route Handlers
- We can pass multiple callbacks to route handlers.
- It is a common pattern to protect specific route (ex: verify/check for something such as auth).
### Example
```js
// Define Middleware
const verifyPassword = (req, res, next) => {
	const { password } = req.query;
	if (password === "chickennugget") {
		next();
	}
	res.send("Sorry you need a password!");
};

// Route
app.get("/secret", verifyPassword, (req, res) => {
	res.send("My Secret Is: blahblahblah");
});
```

## Reference
[Middleware - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Middleware)  
[Using Express middleware](http://expressjs.com/en/guide/using-middleware.html#using-middleware)  
[Writing middleware for use in Experss apps](https://expressjs.com/en/guide/writing-middleware.html)
