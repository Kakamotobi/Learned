# Express
- Framework for creating servers.

## What It Does
  - Start up a server to listen for requests.
  - Parse incoming requests.
    - i.e., turn request(text information) into object.
  - Match those requests to particular routes.
    - Hence, indicating what codes to run for particular requests.
  - Craft http response and the associated content.

## Basics
- **`const app = express();`**
  - Execute express package and save to a variable.
- **`.use(callbackFunction)`**
  - This callback will run every time ANY request hits the server.
- **`.listen(port, callbackFunction)`**
  - This callback will run when the app is running on the indicated port.
  - It will start listening for any incoming requests.
- localhost:3000
  - localhost is a reference to this machine.
  - 3000 is a port number on this machine.
- Port
  - Addresses for connections on the machine.
  - Put different servers on different ports so that they get their own entrance/tunnel to the machine, resulting in separate traffic.
  - Frequently used port numbers: 3000, 8080.

## Example
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
