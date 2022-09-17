# Session Management

## Table of Contents
- [Cookie and Session](#cookie-and-session)

## Cookie and Session
### [Cookie](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Client-side-Storage.md#cookies)
- **A Cookie is a small piece of data that a server creates and sends (in the response headers) to the client.**
- Once the browser receives a Cookie, the Cookie is automatically sent along with all subsequent requests to the server.
  - No need to write code for this, as it is the HTTP standard.
- *Cookies are stored on the browser.*
#### Properties
- **Domain:** the backend that created the Cookie.
  - The browser saves and sends domain-specific Cookies.
- **Path:** the URL.
- **Expires / Max-Age:** the expiration date of the Cookie.
  - If unspecified, the Cookie defaults to become a Session Cookie (expires when the client shuts down).
  - Default Max-Age is 14 days.
  - Example
    ```js
    app.use(
      session({
        secret: "thisIsASecret!",
        cookie: {
          maxAge: 20000,
        },
        store: MongoStore.create({ mongoUrl: "mongodburl" }),
      });
    );
    ```
### Session
- **A Session is a mechanism in which the server can identify and recognize a client, with the help of Cookies.**
- **Session Store**
  - Lives on the server.
  - Contains each client's Session ID, and other information about the Session.
- The Session ID acts as a key that the server issues to the client, which the client then can use to access information on the server.
  - ***Session IDs are sent via Cookies to the clients.***
- **Session Secret:** a piece of string that is used to encrypt Cookies.
  - Cookies are encrypted so that the server knows that it was the one that sent the Cookie to the client.
  - This reduces hackers' ability to steal the Cookie and pretend to be the user (Session hijacking).
  - `secret` should not be revealed in the codebase.
#### `express-session` Session
- **The Session Store for the specific client can be accessed through the request object (`req.session`).**
- If the user provides valid login credentials, add information to the Session Store (update the state of the Session) in order to let the server know that the user has been verified and is logged in (therefore, send user-tailored content).
  - `req.session.isLoggedIn = true`, `req.session.user = user`.
  - In a controller.
- Through the use of `res.locals`, views can have access to the login information.
  - Sync `res.locals` with information on the Session object.
  - `res.locals.isLoggedIn = !!req.session.isLoggedIn`, `res.locals.loggedInUser = req.session.user`.
  - In a middleware.
#### `connect-mongo` Session Store
- A MongoDB based Session Store.
  - i.e. the sessions are stored on MongoDB and not on memory (default).
- Save the Session in a database so that users are logged in despite server restarts.
- Simply provide the MongoDB URL to create a Session Store.
  - The DB URL should not be revealed in the codebase.
### Session Flow (default) Illustration
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/session-and-cookie.png" alt="Session and Cookie Default Flow Illustration" width="80%" />
</p>

- When the client makes an initial request to the server (usually a page GET request), the server opens up a Session and generates a Session ID for the client.
  - The Session Data (incl. Session ID) is stored in the server (in memory by default; unless otherwise connected to a DB).
  - Then, the server puts only the Session ID in a Cookie, and sends the response to the client.
- The client receives the Cookie with the Session ID and saves it.
  - This Cookie is automatically sent along with subsequent requests to the server.
- The client sends a request and Cookie to the server.
- The server receives the client's request and Cookie.
  - The server checks the client's Session ID against the Sessions that it is keeping track of, and then sends an appropriate response.
- The client receives the response.
### Issues to Consider
- You may not wish to create and save sessions for EVERY visitor.
  - This can be expensive for the DB.
  - It may be a better idea to do that for only users that are logged in (exclude anonymous users, bots, etc).
  - Ex: configure session to `resave: false` and `saveUninitialized: false`.
#### Session Flow Illustration
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/session-and-cookie-selective.png" alt="Session and Cookie Selective Flow Illustration" width="80%" />
</p>

- Only keep track of sessions for users that log in.

## Reference
[Session cookies concepts - IBM](https://www.ibm.com/docs/en/sva/9.0?topic=cookies-session-concepts)  
