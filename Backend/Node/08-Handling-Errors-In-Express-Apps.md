# Handling Errors in Express Apps

## Table of Contents
- [Express' Built-In Error Handler](#express-built-in-error-handler)
- [Defining Custom Error handlers](#defining-custom-error-handlers)
- [Handling Async Errors](#handling-async-errors)

## Express' Built-In Error Handler
- Express handles all errors encountered inside of any routes and middlewares, and responds with a default 500 status code and an html page.
- Upon encountering an error, Express will catch it an drespond with its own built-in error handling functionality.
- The stack trace only shows in development mode.

## Defining Custom Error Handlers
- We can define error-handling middleware functions just like any other middleware, except that error-handling functions have four arguments (`err`, `req`, `res`, `next`) instead of three.
  - Override the default error handling (500 status code and html template with stack trace).
- The `err` object needs to be passed in to `next()`.
  - Then, Express will consider the current request as being an error and skip any remaining non-error handling routing and middleware functions.
  - Pass on to the next error handler.
### Example
```js
app.use((err, req, res, next) => {
	console.log("**************************")
	console.log("**********Error***********")
	console.log("**************************")
	console.log(err);
	next(err);
});
```
### Defining Custom Error Class
- Respond with some status code and corresponding message.
```js
// AppError.js

class AppError extends Error {
	constructor(message, status) {
		super();
		this.message = message;
		this.status = status;
	}
}

module.exports = AppError;

```
```js
// index.js

const express = require("express");
const app = express();

constAppError = require("./AppError.js");

// -----Routes----- //
app.get("/error", (req, res) => {
  chicken.fly();
});

app.get("/secret", (req, res) => {
  throw new AppError("Password Required", 401);
}

app.get("/admin", (req, res) => {
	throw new AppError("You're not an Admin!", 403);
});

// -----Error Handling Middleware----- //
app.use((err, req, res, next) => {
  const { status = 500, message = "Something Went Wrong!" } = err;
  res.status(status).send(message);
});

// -----Port----- //
app.listen(3000, () => {
  console.log("Listening on Port 3000");
});
```

## Handling Async Errors
- For errors returned from asynchronous functions invoked by route handlers and middleware, you must pass them to the `next()` function, where Express will catch and process them.
	- In an async function, in order to have Express catch and process errors, we have to call `next()` manually.

```js
// AppError.js

class AppError extends Error {
  constructor(message, status) {
    super();
    this.message = message;
    this.status = status;
  }
}

module.exports = AppError;
```
```js
//index.js

const express = require("express");
const app = express();

constAppError = require("./AppError.js");

const categories = ["fruit", "vegetable", "dairy"];

// -----Routes----- //
app.get("/products", async (req, res) => {
  const { category } = req.query;
  if (category) {
    const products = await Product.find({ category });
    res.render("products/index", { products, category });
  } else {
    const products = await Product.find({});
    res.render("products/index", { products, category: "All" });
  }
});

app.get("/products/new", (req, res) => {
  res.render("products/new", { categories });
});

app.post("/products", async (req, res) => {
  const newProduct = new Product(req.body);
  await newProduct.save();
  res.redirect(`/products/${newProduct._id}`);
});

app.get("/products/:id", async (req, res, next) => {
  const { id } = req.params;
  const product = await Product.findById(id);
  if (!product) {
    next(new AppError("Product Not Found", 404)); // Handling async error
  }
  res.render("products/show", { product });
});

app.get("/products/:id/edit", async (req, res, next) => {
  const { id } = req.params;
  const product = await Product.findById(id);
  if (!product) {
    next(new AppError("Product Not Found", 404)); // Handling async error
  }
  res.render("products/edit", { product, categories });
});

// -----Error Handling Middleware----- //
app.use((err, req, res, next) => {
  const { status = 500, message = "Something Went Wrong!" } = err;
  res.status(status).send(message);
});

// -----Port----- //
app.listen(3000, () => {
  console.log("Listening on Port 3000");
});
```
