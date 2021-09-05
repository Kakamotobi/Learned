# Handling Errors in Express Apps

## Table of Contents
- [Express' Built-In Error Handler](#express-built-in-error-handler)
- [Defining Custom Error handlers](#defining-custom-error-handlers)
- 

## Express' Built-In Error Handler
- Express handles all errors encountered inside of any routes and middlewares, and responds with a default 500 status code and an html page.
- Upon encountering an error, Express will catch it an drespond with its own built-in error handling functionality.
- The stack trace only shows in development mode.

## Defining Custom Error Handlers
- We can define error-handling middleware functions just like any other middleware, except that error-handling functions have four arguments (`err`, `req`, `res`, `next`) instead of three.
- 

