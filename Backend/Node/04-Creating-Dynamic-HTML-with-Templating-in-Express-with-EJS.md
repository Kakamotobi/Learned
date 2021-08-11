# Creating Dynamic HTML with Templating in Express with EJS

## Table of Contents
- [Templating](#templating)
- [Embedded JavaScript](#embedded-javascript-ejs)
- [Configuring Express for EJS](#configuring-express-for-ejs)
- [EJS Interpolating Syntax](#ejs-interpolating-syntax)
- [Passing Data to Templates](#passing-data-to-templates)
- [Conditionals in EJS](#conditionals-in-ejs)
- [Loops in EJS](#loops-in-ejs)
- [Serving Static Assets in Express](#serving-static-assets-in-express)
- [EJS and Partials/Includes](#ejs-and-partialsincludes)
- [Template Demo Example](#template-demo-example)

## Templating
- Allows us to define a preset "pattern" for a webpage, that we can dynamically modify.
  - Instead of creating every single page one at a time (ex: all the subreddits), templates are used.
  - Instead of writing static HTML code that always stays the same, we can embed information and logic. Therefore, we can repeat parts of a template over and over again (ex: using loop).
    - Ex: show, allow certain features depending on whether the user is logged in or not.
    - Ex: define a single "search" template that displays all the results for a given search term. We don't know what the term is or how many results there are ahead of time. The webpage is created on the fly.
- Templating Tools: EJS, Handlebars, etc.

## Embedded JavaScript (EJS)
- Embedding actual JS in templates.
- Runs through the template, evaluates it, and in any places where it sees JS, it runs the JS and spits out HTML.
- https://ejs.co/

## Configuring Express for EJS
### Process
- Setup package.json file and Express.
- `app.set("view engine", "ejs")`
  - Have to indicate to Express that EJS is the templating engine that we will use.
- `npm install ejs`
  - Install EJS.
  - *Don't have to `require("ejs")` because `app.set(propertyName, value)` did it for us.*
- `mkdir views` in current working directory.
  - *By default, when we create a new Express app and use a some view engine, Express assumes that our views/templates exist in a directory called "views".*
  - Setting the views directory so that the file could be run from anywhere in the terminal.
    - The views folder's path: `process.cwd() + "/views"`
    - `const path = require("path")`
      - Require Path that is built in to Express.
    - `app.set("views", path.join(__dirname, "/views"));`
      - *Join the full path to the index.js file with "/views".*
      - `__dirname` references the absolute path starting from the root until where the file (ex: index.js) is located in.
      - Now, add the relative path pointing to the folder where the index.js file is in, to start the server from any directory.
      - Ex: `nodemon previousFolder/index.js`
- `touch home.ejs` in the views folder.
  - Create .ejs files in the views folder.
  - By default, EJS looks for .ejs files in the views folder.

## EJS Interpolating Syntax
- `<%=` `%>`
  - Outputs the value into the template (HTML escaped)
  - *Anything in between will be treated as JS.*
  - Ex: `<%= 5 + 5%>` // turns out as 10 in the template.
- `<%` `%>`
  - 'Scriptlet' tag, for control-flow, no output.
  - *Allows us to embed JS without the result actually be added to the template.*
  - i.e., we can add JS logic without having anything be rendered.
- `<%-` `%>`
  - Outputs the unescaped value into the template.
  - i.e., if there is HTML that we're outputing, it is not going to escape it.
  - "Escaping" content means we're treating it as a string (not as HTML and rendering it).

## Passing Data to Templates
- **Generally, remove as much logic from the templates since templates are best to simply display things.**
- So, generate logic, values, etc. first and then pass it as an object to the templates.
### Example
```js
// index.js

app.get("/random", (req, res) => {
  const randNum = Math.floor(Math.random() * 10) + 1;
  // The object will be passed through and made available to the template "random.ejs".
  res.render("random.ejs", {randNum: randNum});
});
app.listen(3000, () => {
  console.log("Listening on Port 3000!");
});
```
```ejs
// random.ejs

<body>
  <h1>Your random number is: <%= randNum %></h1>
</body>
```

## Conditionals in EJS
### Example 1 
```js
// index.js 

// Random Route
app.get("/random", (req, res) => {
  const randNum = Math.floor(Math.random() * 10) + 1;
  // randNum will be available in the template "random.ejs" under the name "randNum".
  // The object will be passed through to the template.
  res.render("random.ejs", { randNum: randNum });
});
```
```ejs
// random.ejs

<body>
  <h1>Your random number is: <%= randNum %></h1>

  <% if (randNum % 2 === 0) { %>
    <!-- h2 is only rendered if this is true -->
    <h2>That is an even number!</h2>
  <% } else { %>
    <h2>That is an odd number!</h2>
  <% } %>
</body>
```
### Example 2
```ejs
// random.ejs

<body>
  <h1>That number is: <%= randNum % 2 === 0 ? "Even" : "Odd" %></h1>
</body>
```

## Loops in EJS
### Example
```js
//index.js

// Cats Route
app.get("/cats", (req, res) => {
  // Pretend this is a cats database
  const cats = ["Blue", "Rocket", "Monty", "Stephanie", "Winston"];
  res.render("cats.ejs", { cats: cats });
});
```
```ejs
// cats.ejs

<body>
  <h1>All The Cats</h1>
  
  <!-- ul full of li (cats) -->
  <ul>
    <% for (let cat of cats) { %>
    <li><%= cat %></li>
    <% } %>
  </ul>
</body>
```

## Serving Static Assets in Express
- Static Assets/Files
  - Ex: images, CSS files, JS files.
- "Served"
  - Accessible directly.
  - Ex: localhost:3000/style.css
### Process
- `mkdir public` in root directory.
  - Create a folder(s) called "public" that we want to serve our static assets from.
- `app.use(express.static("public"));`
  - Serve static assets/files from a directory named "public".
  - Now, we can load files in the "public" folder.
- Refer to the static assets in the templates to apply them.
  - Ex: `<link rel="stylesheet" href="/style.css">`
    - Reference the path inside of the "public" directory to get to that asset.
    - No need to reference the path "public/" because we are going to serve the contents of the directory; not the directory itself.
### Reference
[Serving static files in Express](https://expressjs.com/en/starter/static-files.html)

## EJS and Partials/Includes
- `<%- include("pathToTheTemplate") %>`
  - A way of including a template in other templates.
  - i.e., sub-template inside of another template.
- Ex: Common links, scripts, HTML, navbar, etc.
### Process
- Create a "partials" directory in the "views" directory.
- Cut the common parts of templates and move it into a new template.
- Save the new template in the "partials" directory.
- Include the "partial" template in the other templates.
  - Ex: `<%- include("partials/head.ejs") %>`

## Template Demo Example
```js
// index.js

const express = require("express");
const app = express();
const path = require("path");
// Fake Data for Demo
const fakeData = require("./fakeData.json")

// -----Static Assets Setup----- //
app.use(express.static(path.join(__dirname, "/public")));

// -----EJS Setup----- // 
app.set("view engine", "ejs");
app.set("views", path.join(__dirname, "/views"));

// -----Route----- //
app.get("/r/:subreddit", (req, res) => {
  // Destructure ":subreddit" from path.
  const { subreddit } = req.params;
  // Use the destructured ":subreddit" to find it in fakeData.json and save it.
  const data = fakeData[subreddit];
  // If data exists, display it. If not, display notfound.ejs.
  if (data) {
    res.render("subreddit.ejs", { ...data });
  } else {
    res.render("notfound.ejs", { subreddit });
  }
});

// -----Port----- //
app.listen("3000", () => {
  console.log("Listening on Port 3000!");
});
```
```ejs
// subreddit.ejs

<body>
  <%- include("partials/navbar.ejs") %>
  
  <h1>Browsing The <%= name %> Subreddit</h1>
  <h2><%= description %></h2>
  <p><%= subscribers %> Total Subscribers</p>
  <hr />

  <% for (let post of posts) { %>
  <article>
    <h3><%= post.title %> - <%= post.author %></h3>
    <% if (post.img) { %>
    <img src="<%= post.img %>" alt="" />
    <% } %>
  </article>
  <% } %>
</body>
```
```ejs
// notfound.ejs

<body>
  <%- include("partials/navbar.ejs") %>
  <h1>Sorry. We couldn't find the <%= subreddit %> subreddit.</h1>
</body>
```
```ejs
// navbar.ejs

<nav></nav>
```
