# Express
- Framework for creating full-featured web servers that can receive and respond to network requests.

## Table of Contents
- [What It Does](#what-it-does)
- [Starting Up the Server](#starting-up-the-server)
- [Request and Response Objects](#request-and-response-objects)
- [Routing](#routing)
- [Working with Query Strings](#working-with-query-strings)
- [Server Example](#server-example)

## What It Does
  - Start up a server to listen for HTTP requests.
  - Parse incoming HTTP requests.
    - i.e., turn request(text information) into JS object.
  - Match those requests to particular routes.
    - Hence, indicating what codes to run for particular requests.
  - Craft http response and the associated content.

## Starting Up the Server
- **`npm init`**
  - Make package.json file.
- **`npm install express`**
  - Download "node_modules" for Express.
- **`const express = require("express");`**
  - Return the `exports` object from Express.
- **`const app = express();`**
  - Execute Express package and save to a variable.
  - `app` is an object that describes all the things that the web server can do.
- **`app.use([path,] callback [,callback...])`**
  - This callback will run every time ANY request hits the server.
  - Handles every incoming request the exact same way.
- **`app.listen(port, callback)`**
  - This callback will run when the app is running on the indicated port.
  - It will start listening for any incoming requests on that port.
  - Ex: `app.listen(3000, () => { console.log("Listening on Port 3000") })`
- localhost:3000
  - localhost is a reference to this machine.
  - 3000 is a port number on this machine.
- Port
  - Addresses for connections on the machine.
  - Put different servers on different ports so that they get their own entrance/tunnel to the machine, resulting in separate traffic.
  - Frequently used port numbers: 3000, 8080.

## Request and Response Objects
- On every incoming request, Express gives us access to these two parameters.
### Request Object
> The `req` object represents the incoming HTTP request and has properties for the request query string, parameters, body, HTTP headers, and so on.
- *HTTP requests are text information but Express takes it and turns it into an object.*
### Response Object
> The `res` object represents the outgoing HTTP response that an Express app sends when it gets an HTTP request.
#### `res.send()`
- Generates and sends the HTTP response content.
- Accepts a string, a string containing HTML tags, a JS object (JSON representation of it), etc.
  - HTML tags are rendered in the browser.
- *Once we send something back, we're done for that one request because we can't have an HTTP request that gets more than one response.*
### Example
```js
app.get("/user/:id", (req, res) => {
  res.send("user " + req.params.id)
})
```

## Routing
> Refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).
> Each route can have one or more handler functions, which are executed when the route is matched.
- Routing incoming request and a path, to a relevant outgoing response.
### Defining Routes
- **`app.METHOD(PATH, HANDLER)`**
  - `METHOD` is an HTTP request method, in lowercase.
  - `PATH` is a path on the server.
  - `HANDLER` is the function executed when the route is matched.
- Paths
  - `/`
    - Home/Root Route
  - `*`
    - Catch-all for the indicated request type.
    - Used to make a generic response for all particular requests of a particular request type.
    - *Always place at the end of the other routes for the same request type.*
#### Examples
- Home/Root Route
  ```js
  app.get("/", (req, res) => {
    res.send("Here is your response")
  })
  ```
- Other Routes
  ```js
  // Route for GET request on /dogs.
  app.get("/dogs", (req, res) => {
    res.send("Woof!");
  });

  // Route for POST request on /dogs.
  app.post("/dogs", (req, res) => {
    res.send("Got your POST request")
  })
  
  // Generic response for all GET requests.
  app.get("*", (req, res) => {
    res.send("I don't know that path!");
  });
  ```
### Path Parameters/Variables in Routes
- Define a generic pattern, instead of manually hardcoding all routes for each exact match.
- `:` indicates a variable.
  - Ex: `/r/:subreddit`
- `req.params`
  - Contains the key-value pairs from the path string.
#### Examples
```js
app.get("/r/:subreddit", (req, res) => {
  const { subreddit } = req.params;
  res.send(`<h1>Browsing on the Subreddit: ${subreddit}!</h1>`);
});

app.get("/r/:subreddit/:postId", (req, res) => {
  const { subreddit, postId } = req.params;
  res.send(`<h1>Viewing POST ID: ${postId} on the Subreddit: ${subreddit}!</h1>`);
});
```

## Working with Query Strings
- Query String
  - Portion of the URL that comes after a question mark, to which we can include information and key-value pairs.
  - Use Ex: search, sort.
### `req.query`
> Object containing a property for each query string parameter in the route.
### Example
```js
app.get("/search", (req, res) => {
  const { q } = req.query;
  if (!q) {
    res.send(`<h1>Nothing found if nothing searched</h1>`);
  } else {
      res.send(`<h1>Search results for: ${q}</h1>`);
  }
});
```

## Server Example
```zsh
// Terminal

node init -y
npm install express
```
```js
// index.js

const express = require("express");
const app = express();

app.get("/r/:subreddit/:postId", (req, res) => {
  const { subreddit, postId } = req.params;
  res.send(`<h1>Viewing Post ID: ${postId} on the Subreddit: ${subreddit}!</h1>`);
});

app.get("/search", (req, res) => {
  const { q } = req.query;
  if (!q) {
    res.send("Nothing found if nothing searched!");
  } else {
    res.send(`Search results for: ${q}`);
  }
});

app.get("*", (req, res) => {
  res.send("I don't know that path!")
});

app.listen(3000, () => {
  console.log("Listening on Port 3000!");
});
```

```zsh
// Terminal

node index.js
```
```
// Browser

localhost:3000/r/chickens/7777 // send request.

// Response
Viewing Post ID: 7777 on the Subreddit: chickens! // response received.
```

## Reference
[Express - Node.js web application framework](https://expressjs.com/)  
[Express Application Methods](https://expressjs.com/en/api.html#app)
