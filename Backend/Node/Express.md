# Express
- Framework for creating servers.

## What It Does
  - Start up a server to listen for HTTP requests.
  - Parse incoming HTTP requests.
    - i.e., turn request(text information) into JS object.
  - Match those requests to particular routes.
    - Hence, indicating what codes to run for particular requests.
  - Craft http response and the associated content.

## Starting Up the Server
- **`const app = express();`**
  - Execute express package and save to a variable.
- **`.use([path,] callback [,callback...])`**
  - This callback will run every time ANY request hits the server.
- **`.listen(port, callback)`**
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
### Response Object
> The `res` object represents the outgoing HTTP response that an Express app sends when it gets an HTTP request.
#### `res.send()`
  - Generates and sends the HTTP response.
  - Accepts a string, a string containing HTML tags, a JS object (JSON representation of it), etc.
    - HTML tags are rendered in the browser.
### Example
```
app.get("/user/:id", (req, res) => {
  res.send("user " + req.params.id)
})
```

## Routing
> Refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).
> Each route can have one or more handler functions, which are executed when the route is matched.
### Defining Routes
- **`app.METHOD(PATH, HANDLER)`**
  - `METHOD` is an HTTP request method, in lowercase.
  - `PATH` is a path on the server.
  - `HANDLER` is the function executed when the route is matched.
### Example
```
app.get("/", (req, res) => {
  res.send("Here is your response")
})
app.post("/", (req, res) => [
  res.send("Got your POST request")
})
```

## Server Example (Need to Update!)
```
<Terminal>
node init // create package.json
npm install express

<index.js>
const express = require("express");

app.use(() => {
  console.log("Received a new request!");
});

app.listen(3000, () => {
  console.log("Listening on Port 3000!");
});

<Terminal>
node index.js

<Browser>
localhost:3000 // send request
```

## Reference
[Express - Node.js web application framework](https://expressjs.com/)  
[Express Application Methods](https://expressjs.com/en/api.html#app)
