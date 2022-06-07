# Authentication

## Table of Contents
- [HTTP is Stateless](#http-is-stateless)
- [Cookie and Session](#cookie-and-session)
  - [Cookie](#cookie)
  - [Session](#session)
  - [Session Flow Illustration](#session-flow-illustration)

## HTTP is Stateless
> A stateless protocol is a communication protocol in which the receiver must not retain session state from previous requests. | Wikipedia

- **HTTP is stateless because every single request that the server receives is independent from one another.**
  - i.e. there is no link between any requests even if sent by the same client.
  - i.e. the server does not remember the client.
- After the HTTP request is fulfilled with an HTTP response, there is no more live connection between the client and the server.
- This raises the question, how do you allow stateful sessions in a stateless protocol?
  - i.e. how do you present tailored content to each user?
- Therefore, in order to maintain session state between a client and a server, there needs to be a way for the client to identify itself and the server to verify the client.
  - This is done through **Sessions** and **Cookies**.
  - For every request, the client has to re-identify itself by sending the Cookie, which holds the Session ID, back to the server.
    - *This Cookie can only be sent to the server that generated the Cookie.*

## Cookie and Session
### [Cookie](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Client-side-Storage.md#cookies)
- **A Cookie is a small piece of data that a server creates and sends (in the response headers) to the client.**
- Once the browser receives a Cookie, the Cookie is automatically sent along with all subsequent requests to the server.
  - No need to write code for this, as it is the HTTP standard.
- *Cookies are stored on the browser.*
### Session
- **A Session is a mechanism in which the server can identify and recognize a client, with the help of Cookies.**
- *The Session Object lives on the server.*
- The Session Object contains each client's Session ID.
  - The Session ID acts as a key that the server issues to the client, which the client then can use to access information on the server.
- ***Session IDs are sent via Cookies to the clients.***
#### `express-session` Example
- The Session object can be accessed through the request object (`req.session`).
- If the user provides valid login credentials, add information to the Session object (update the state of the Session) in order to let the server know that the user has been verified and is logged in (therefore, send user-tailored content).
  - `req.session.isLoggedIn = true`, `req.session.user = user`.
  - In a controller.
- Through the use of `res.locals`, views can have access to the login information.
  - Sync `res.locals` with information on the Session object.
  - `res.locals.isLoggedIn = !!req.session.isLoggedIn`, `res.locals.loggedInUser = req.session.user`.
  - In a middleware.
### Session Flow Illustration
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/session-and-cookie.png" alt="Session and Cookie Flow Illustration" width="80%" />
</p>

- When the client makes an initial request to the server (usually a page GET request), the server opens up a Session and generates a Session ID for the client.
  - The Session Data (incl. Session ID) is stored in the server.
  - Then, the server puts only the Session ID in a Cookie, and sends the response to the client.
- The client receives the Cookie with the Session ID and saves it.
  - This Cookie is automatically sent along with subsequent requests to the server.
- The client sends a request and Cookie to the server.
- The server receives the client's request and Cookie.
  - The server checks the client's Session ID against the Sessions that it is keeping track of, and then sends an appropriate response.
- The client receives the response.

## Reference
[Stateless protocol - Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)  
[Session cookies concepts - IBM](https://www.ibm.com/docs/en/sva/9.0?topic=cookies-session-concepts)  
