# Authentication

## Table of Contents
- [HTTP is Stateless](#http-is-stateless)
- [Cookie and Session](#cookie-and-session)
  - [Cookie](#cookie)
  - [Session](#session)
  - [Session Flow Illustration](#session-flow-illustration)
  - [Issues to Consider](#issues-to-consider)
- [Open Authorization (OAuth)](#open-authorization-oauth)

## HTTP is Stateless
> A stateless protocol is a communication protocol in which the receiver must not retain session state from previous requests. | Wikipedia

- **HTTP is stateless because every single request that the server receives is independent from one another.**
  - i.e. there is no link between any requests even if sent by the same client.
  - i.e. the server does not remember the client.
- After the HTTP request is fulfilled with an HTTP response, there is no more live connection between the client and the server.
- This raises the question, how do you allow stateful sessions in a stateless protocol?
  - i.e. how do you present tailored content to each user?
- Therefore, in order to maintain session state between a client and a server, there needs to be a way for the client to identify itself and the server to verify the client.
  - This can be done through the use of **Sessions** and **Cookies**.
  - For every request, the client has to re-identify itself by sending the Cookie, which holds the Session ID, back to the server.
    - *This Cookie can only be sent to the server that generated the Cookie.*

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

- Only keep track of Sessions for users that log in.

## Open Authorization (OAuth)
### What is OAuth?
> The OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf. | IETF

> OAuth ("Open Authorization") is an open standard for access delegation, commonly used as a way for internet users to grant websites or applications access to their information on other websites but without giving them the passwords. | Wikipedia

- OAuth is convenient and more secure than creating a new account or entering your password into a third-party application.
- For example, even if a site that you used was hacked, if you used Google OAuth, your credentials are likely not compromised because you have no credentials stored in that site.
### How it works
- The third-party application first has to acquire two tokens from the OAuth provider (Ex: Google).
  - The two tokens: 
    - "Consumer Key" a.k.a. "Client ID", etc.
    - "Consumer Secret" a.k.a. "Client Secret", etc.
  - These two tokens connect the third-party application and the OAuth provider.
    - They are required to be included in OAuth request (often in parameters).
- On the third-party application, the user clicks "Continue with Google".
  - The third-party application redirects the user to Google.
  - If the user is not logged in to Google, the user is required to log in to Google.
    - ***The user is logging in to Google and not the third-party application.***
  - Google confirms with the user the information (on their Google account) that the third-party application will get access to.
- If the user confirms, an Access Token is granted to the third-party application that allows the application to access the allowed information.
### OAuth Flow
1. Users are redirected to request their ___ identity.
    - User provides the login credentials for ___.
    - User accepts to share data with the application/site.
2. Users are redirected back to the application/site by ___.
    - Users are returned to the site with an access token.
    - This token expires very quickly.
3. The application/site accesses the ___ API with the user's access token.
#### Example
1. Request a user's GitHub identity.
    - `GET https://github.com/login/oauth/authorize`
    - Include parameters to configure the authorization.
      - `client_id`: the ID that GitHub issued when the application was registered to GitHub.
      - `scope`: represents the informations that the application wants from the user's GitHub identity.
2. GitHub redirects users back to the application/site.
    - `GET authorizationCallbackUrl`.
      - If the user accepted the authorization, GitHub redirects the user to the Authorization callback URL, which was registered to GitHub, along with a temporary code (expires in 10 minutes) in the parameter (`req.query.code`).
    - `POST https://github.com/login/oauth/access_token`
      - Exchange the code for an access token.
      - Required Parameters:
        - `client_id`
        - `client_secret`
        - `code`
    - The Access Token is received from the response to the POST request.
    - *The Access Token only gives access to the informations mentioned in `scope`.*
3. Use the access token to access the API.
    - `GET https://api.github.com/user`
    - Required Headers:
      - `Authorization: "token OAUTH-TOKEN"`
        - OAUTH-TOKEN: the access token.
### Issues
- How should we tell whether the user is who they claim to be they are?
  - Scenario 1: User created an account using an email and password. User tries to login with GitHub OAuth.
    - The email used in GitHub is also the same email that was used to create the account.
    - But the password registered for that email was not provided (since GitHub was used).
      - Allow login (has the user proven to be the user)?
      - Tell the user to login using email and password?
        - Ex: "There is already an account by that email".
  - Scenario 2: User created an account using GitHub OAuth (no password in DB). User tries to login with email and password (which does not exist in DB).
    - Tell the user to login using GitHub?
      - Ex: "There is an account by that email but no password. Login with GitHub".

## Reference
[Stateless protocol - Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)  
[Session cookies concepts - IBM](https://www.ibm.com/docs/en/sva/9.0?topic=cookies-session-concepts)  

[RFC 6749 - The OAuth 2.0 Authorization Framework - IETF](https://datatracker.ietf.org/doc/html/rfc6749)  
[OAuth - Wikipedia](https://en.wikipedia.org/wiki/OAuth#:~:text=OAuth%20(%22Open%20Authorization%22),without%20giving%20them%20the%20passwords.)  
[Understanding OAuth: What Happens When You Log Into a Site with Google, Twitter, or Facebook](https://lifehacker.com/understanding-oauth-what-happens-when-you-log-into-a-s-5918086)  
[Authorizing OAuth Apps - GitHub Docs](https://docs.github.com/en/developers/apps/building-oauth-apps/authorizing-oauth-apps)  
