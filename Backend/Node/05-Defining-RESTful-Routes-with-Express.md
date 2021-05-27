# Defining RESTful Routes with Express

## Table of Contents
- [HTTP Requests](#http-requests)
- [Defining POST Routes in Express](#defining-post-routes-in-express)
- [Parsing the POST "Request Body"](#parsing-the-post-request-body)
- [Representational State Transfer (REST)](#representational-state-transfer-rest)
- [Express Redirects](#express-redirects)
- [uuid Package](#uuid-package)
- [method-override Package with Express](#method-override-package-with-express)
- [CRUD Implementation of RESTful Routes Example](#crud-implementation-of-restful-routes-example)

## HTTP Requests
### GET
- Used to search/retrive information.
  - Ex: web page, top post, etc.
- Query data is sent via query string.
- Information is plainly visible in the URL.
  - Hence, limited amount of data can be sent.
- Nothing should be created, deleted, or updated in the backend.
### POST
- Used to post data to the server.
- Data is sent via "request body" (not query string).
  - Data that is sent (from the client) is not part of the query string; it's a payload of the request.
  - i.e., data is there but is not visible in the URL.
- Can send any sort of data (JSON).
- Conventionally used to create things (sending bunch of data).
  - Ex: signing up, commenting, uploading, etc.
- Create, delete, update in the backend.
### PUT
- Used to replace all current representations of the target resource with the request payload.
  - Ex: payload includes all the components of a comment (id, username, comment) to replace existing representations.
### PATCH
- Used to apply partial modifications to a resource using the payload.

## Defining POST Routes in Express
- Receiving and handling POST requests in Express.
### Example
```
<<index.js>>
const express = require("express");
const app = express();

app.post("/tacos", (req, res) => {
  res.send("POST /tacos response!");
});

app.listen(3000, () => {
  console.log("Listening on Port 3000!");
});
```

## Parsing the POST "Request Body"
- Getting the data out of the POST request and using it.
- **`req.body`**
  - Payload of incoming request.
  > Contains key-value pairs of data submitted in the request body. By default, it is `undefined`, and is populated when you use body-parsing middleware such as body-parser and multer.
    - `undefined` boils down to the fact that data can be sent in different formats.
- So, explicitly indicate how Express should parse request bodies.
  - If the different formats of data are not indicated to be parsed, that format will not be parsed.
### Example
```
<<index.js>>
// For parsing URL encoded form data.
app.use(express.urlencoded({ extend: true}));

// For parsing JSON.
app.use(express.json());
```

## Representational State Transfer (REST)
> Architectural style for distributed hypermedia systems.
- A set of guidelines for how a client and server should communicate and perform CRUD operations on a given resource.
- "RESTful" means that it complies with the REST guidelines.
- **Main Idea:** treat data on the server-side as resources that can be CRUDed.
- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
  - 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.
### Architectural Constraints
- Client-Server Architecture
  - Client and Server are separated, and we could swap in a different client.
  - It doesn't matter where the request comes from because the client and server are not linked.
  - Ex: making requests from either the browser or from Postman works.
- Stateless
  - HTTP is a stateless protocol.
  - Every request is on its own in a vacuum.
    - Ex: it doesn't have access to the request that came before.
- Uniform Interface
  - Every RESTful system has a uniform interface, which most of the time, consists of some consistent URL pattern matched with different HTTP verbs/methods.
  - Combine the base URL with different HTTP verbs/methods to expose full CRUD operations over HTTP.
  - Pattern: having a resource name and then matching that path with an HTTP verb/method.
- Cache
- Layered System
- Code-On-Demand
### Example
```
// IG Comment

// Reading a Comment
GET /{ig-comment-id}?fields={fields}

// Updating a Comment
POST /{ig-comment-id}?hide={hide}

// Deleting a Comment
DELETE /{ig-comment-id}
```

## Express Redirects
- **`res.redirect([status,]path)`**
  > Redirects to the URL derived from the specified path, with specified status, a positive integer that corresponds to an HTTP status code . If not specified, status defaults to “302 “Found”.
  - When the browser gets to 302, it follows up and makes a second request (GET) to the specified path.

## uuid Package
- Universally Unique Identifier
### Process
- `npm install uuid`
- `const { v4: uuidv4 } = require("uuid");`
- `uuidv4()`
  - Call to get a new uuid.

## method-override Package with Express
> Lets you use HTTP verbs such as PUT or DELETE in places where the client doesn't support it.
- HTML forms can only send GET or POST requests (cannot send PUT or PATCH or DELETE requests).
### Process
- `npm install method-override`
- `const methodOverride = require("method-override)";`
- `app.use(methodOverride("_method"));`
  - Specify the query string key as a string argument.
- `<form method="POST" action="/resource?_method=DELETE>`
  - Express will treat the form's POST request as a DELETE request by doing this.

## CRUD Implementation of RESTful Routes Example
- *Match HTTP verbs with a base URL (often pluralized resource), to which a unique identifier is added on when appropriate.*
```
// Resource: comment
// Resource Components: username, comment text

// Blueprint
Route Name | Path | Verb | Purpose

Index | /comments | GET | Display all comments
New | /comments/new | GET | Form to create new comment
Create | /comments | POST | Create a new comment
Show | /comments/:id | GET | Details for one specific comment (using ID)
Edit | /comments/:id | GET | Form to edit specific comment (using ID)
Update | /commends/:id | PATCH | Update a specific comment on server
Delete | /commends/:id | DELETE | Delete specific item on server

<<index.ejs>>
<body>
  <h1>Comments</h1>

  <ul>
    <% for(let c of comments) { %>
    <li><%=c.comment%> - <b><%= c.username%></b><a href="/comments/<%= c.id %>">Details</a></li>
    <% } %>
  </ul>

  <a href="/comments/new">New Comment</a>
</body>

<<new.ejs>>
<body>
  <h1>Create a New Comment</h1>

  <form action="/comments" method="POST">
    <section>
      // name attr. is what the data will be sent under when the POST request is sent off.
      <label for="username">Username:</label>
      <input type="text" id="username" placeholder="username" name="username" />
    </section>

    <section>
      <label for="comment">Comment:</label>
      <br />
      <textarea id="comment" cols="26" rows="5" name="comment"></textarea>
    </section>

    <button>Submit</button>
  </form>

  <a href="/comments">Back to Comments</a>
</body>

<<show.ejs>>
<body>
  <h1>Comment ID: <%= comment.id %></h1>

  <h2><%= comment.comment %> - <%= comment.username %></h2>

  <a href="/comments/<%= comment.id %>/edit">Edit Comment</a>
  <form method="POST" action="/comments/<%= comment.id %>?_method=DELETE">
    <button>Delete<Button>
  </form>
  <a href="/comments">Back to Comments</a>
</body>

<<edit.ejs>>
<body>
  <h1>Edit</h1>
  <form method="POST" action="/comments/<%= comment.id %>?_method=PATCH">
    <textarea name="comment" id="" cols="30" rows="10">
      <%= comment.comment %>
    </textarea>
    <button>Update Comment</button>
  </form>
</body>

<<index.js>>
// ----------Setup---------- //
const express = require("express");
const app = express();
const path = require("path");
const { v4: uuidv4 } = require("uuid");
const methodOverride = require("method-override");

app.use(express.urlencoded({ extend: true })); // For parsing url form body request.
app.use(express.json()); // For parsing JSON body request.
app.use(methodOverride("_method")); // For overriding form method.
app.set("view engine", "ejs");
app.set("views", path.join(__dirname, "views"));

// ----------Comments (Fake Database)---------- //
let comments = [
  {
    id: uuidv4(),
    username: "Tom",
    comment: "I like to fight Jerry",
  },
  {
    id: uuidv4(),
    username: "Jerry",
    comment: "I like to fight Tom",
  },
  {
    id: uuidv4(),
    username: "Spike",
    comment: "I like to sleep",
  },
];

// ----------Routes---------- //
// Reading all comments
app.get("/comments", (req, res) => {
    res.render("comments/index.ejs", { comments });
});

// Creating new comment (2 routes needed)
// 1. GET route to serve the form page.
app.get("/comments/new", (req, res) => {
  res.render("comments/new.ejs");
});
// 2. When that form is submitted, its data is sent as a POST request to a different path.
app.post("/comments", (req, res) => {
  const { username, comment } = req.body;
  comments.push({ id: uuidv4(), username, comment });
  res.redirect("/comments");
});

// Details for one specific comment (using ID)
app.get("/comments/:id", (req, res) => {
  const { id } = req.params;
  const comment = comments.find(c => c.id === id);
  res.render("comments/show.ejs", { comment });
});

// Update specific comment (using ID)
// 1. GET route to serve the form
app.get("/comments/:id/edit", (req, res) => {
  const { id } = req.params;
  const comment = comments.find((c) => c.id === id);
  res.render("comments/edit.ejs", { comment });
});
// 2. PATCH route to update comment
app.patch("/comments/:id", (req, res) => {
  const { id } = req.params;
  const originalComment = comments.find((c) => c.id === id);
  const updateComment = req.body.comment;
  originalComment.comment = updateComment;
  res.redirect("/comments");
});

// Delete specific comment
app.delete("/comments/:id", (req, res) => {
  const { id } = req.params;
  const comment = comments.find((c) => c.id === id);
  comments = comments.filter((c) => c.id !== id);
  res.redirect("/comments");
});

// ----------Port---------- //

app.listen(3000, () => {
  console.log("Listening on Port 3000!");
});
```

## Reference
[Express 5.x - API Reference](http://expressjs.com/en/5x/api.html#req.body)  
[Fielding Dissertation: CHAPTER 5: Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_5)  
[[Network] REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)  
[uuid - npm](https://www.npmjs.com/package/uuid)  
[method-override - npm](https://www.npmjs.com/package/method-override)
