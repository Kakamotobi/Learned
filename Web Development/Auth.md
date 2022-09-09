# Auth

## Table of Contents
- [HTTP is Stateless](#http-is-stateless)
- [Authentication vs. Authorization](#authentication-vs-authorization)
  - [Terminology](#terminology)
- [Authentication](#authentication)
  - [Cookie-based (Session Cookie) Authentication](#cookie-based-session-cookie-authentication)
  - [Token-based Authentication](#token-based-authentication)
  - [OpenID Connect(OIDC) Protocol](#openid-connectoidc-protocol)
- [Authorization](#authorization)
  - [OAuth 2.0](#oauth-20)
- [Persistent Authentication](#persistent-authentication)
- [Session Management](#session-management)
  - [Cookie and Session](#cookie-and-session)
    - [Cookie](#cookie)
    - [Session](#session)
    - [Session Flow Illustration](#session-flow-illustration)
    - [Issues to Consider](#issues-to-consider)

## HTTP is Stateless
> A stateless protocol is a communication protocol in which the receiver must not retain session state from previous requests. | Wikipedia

- **HTTP (incl. HTTPS) is stateless because every single request that the server receives is independent from one another.**
  - i.e. there is no link between any requests even if sent by the same client.
  - i.e. the server does not remember the client.
- After the HTTP request is fulfilled with an HTTP response, there is no more live connection between the client and the server.
- This raises the question, how do you allow stateful sessions in a stateless protocol?
  - i.e. how do you present tailored content for each user?
- In order to maintain session state between a client and a server, there needs to be a way for the client to identify itself and the server to verify the client for every request.
  - There are two main ways to achieve this: **Sessions** and **Tokens**.

## Authentication vs. Authorization
| Authentication | Authorization |
| ------------- | ------------- |
| The process of verifying whether or not users are in fact who they claim to be. | The process of verifying whether or not a user has access to what they are trying to access. |
| Verified through valid credentials. | Verified through policies and rules. |
| Usually done before authorization. | Usually done after authentication. |
| Data is transmitted through ID Tokens. | Data is transmitted through Access Tokens. |
| Authentication follows the OpenID Connect Protocol. | Authorization follows the OAuth 2.0 framework. |
### Terminology
- **Resource Owner:** user.
- **Client:** the application that wants to access the Resource Owner's data and perhaps perform an action on behalf of the Resource Owner.
- **Authorization Server:** the application that the Resource Owner has an account on.
- **Resource Server:** the API that the Client wants to use on behalf of the Resource Owner.
  - Sometimes the Authorization Server and the Resource Server can be the same.
- **Redirect URI:** the URL that the Authorization Server will redirect the Resource Owner back to, after granting permission to the client.
  - A.K.A., callback URL.
- **Response Type:** the type of information that the client expects to receive.
  - Ex: authorization code.
- **Scope:** the permisions that the Client wants.
- **Consent:** the Authorization Server verifies with the Resource Owner whether or not they want to give the Client permission.
- **Client ID:** the ID that is used to identify the Client with the Authorization Server.
- **Client Secret:** the secret password that only the Client and the Authorization Server know.
- **Authorization Code:** a shortlived temporary code that the Authorization Server sends back to the Client, which then the Client sends back to the Authorization Server along with the Client Secret in order to receive an Access Token.
- **Access Token:** the key that the Client will use to communicate with the Resource Server.

## Authentication
### Cookie-based (Session Cookie) Authentication
- This is the traditional approach on the web, which requires [session management](#session-management).
- **The authentication state (sessions) is handled on the server.**
#### Flow
1. User submits username and password to the server (HTTP request).
2. The server validates it and creates/stores a **session** in the database. Then, it sends the **session ID** to the client (HTTP response).
    - The session ID is just a unique identifier for a user's login session.
3. The client saves the **session ID** in the browser's **cookies**.
    - The browser's cookies is a place in the browser to save key-value pairs that will automatically be sent back to the server on each subsequent request.
    - Cookies merely act as a means of transport for the session ID.
4. The server receives a subsequent request and checks the database to see if the session is still valid.
    - Now the server can respond back with content for the currently logged in user.
5. A stateful session has been established between the frontend client and the backend server.
    - Upon logging out or inactive for a long time, the login session in the database will be invalidated and the server will instruct the browser to delete the cookie containing the no longer valid session ID.
#### Drawbacks
- This approach can be vulnerable to Cross-Site Request Forgery (CSRF).
- The session ID must be stored in a database or keep it in memory on the server, which can be a bottleneck in production since most cloud applications are scaled horizontally.
### Token-based Authentication
- Use of encrypted security tokens to verify a user's identity.
- **The authentication state (JWT) is handled on the client.**
#### Token Storage on Client and Security
##### Encrypted Http-Only Cookie
- An Http-Only Cookie is a cookie that prevents the frontend code from reading or writing to the cookie header, effectively blocking access to the contents of the cookie (not vulnerable to cross-site scripting(XSS) attacks).
- Upon first authentication, return the cookie instead of the tokens.
- You can set the lifetime on these cookies to limit the logged in state.
- Simply log a user out by deleting the cookie.
- Encrypt the cookie with a key that is stored in the server just in case the user's computer is compromised.
- *Notes*
  - Some OAuth implementations do not support accepting cookies (only support sending via the Authorization header). In this case, after the token is decrypted, use a middleware to extract the token, and then add an Authorization header on the inbound request.
##### CSRF Protection
- Use a CSRF token and API key to encode the JWT to ensure that the requests are actually coming from your application.
#### Flow
1. User submits username and password to the server (HTTP request).
2. The server generates a JWT. Then, it sends the JWT to the client (HTTP response).
    - The JWT is created with a private key on the server.
    - *The server doesn't create a session (no session ID).*
3. The client saves the JWT in the browser's local storage.
    - The JWT will be added to the authorization header on each subsequent request.
    - Ex: `Authorization: Bearer <token>`.
4. The server validates the signed JWT header.
    - There is no need to lookup the database.
5. A stateful session has been established between the frontend client and the backend server.
#### Drawbacks
- Tokens can be hijacked by attackers and it can be difficult to invalidate hijacked tokens.
### OpenID Connect(OIDC) Protocol
> OpenID Connect (OIDC) is an identity layer built on top of the OAuth 2.0 framework. It allows third-party applications to verify the identity of the end-user and to obtain basic user profile information. | Auth0

- It enables a client to establish a login session as well as get some information about the user that is logged in.
- It enables a user to be able to use one login across multiple applications (a.k.a. Single Sign-Ons).
- OIDC cannot work without the underlying OAuth framework.
#### How it Works
- Upon successful authentication, OpenID Connect returns an **ID Token**.
  - An ID token verifies that the user is who they claim to be.
  - ID Tokens are required to be in JWT format.
  - It contains a Header, Payload/Body, and Signature.
    - Header
      - Type of token (JWT).
      - Signing algorithm being used (Ex: SHA256).
    - Payload/Body
      - `"sub"`: the unique identifier for the user.
      - `"aud"`: the client ID of the intended receiver for this ID Token to be used.
      - `"iss"`: the identity provider who created the ID Token.
      - Some information (referred to as "claims") about an entity (user) and other metadata.
    - Signature
      - The result of signing the encoded header, the encoded payload, secret, and the algorithm specified in the header.
      - Ex: `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`
  - The final recepient (`"aud"`) of an ID token is the client.
#### Important Notes
- ID Tokens are NOT meant for authorization. They are strictly for authentication.
- ID Tokens should NOT be sent to an API.
- ID Tokens do NOT contain authorization information (hence, useless to send to an API anyways).
- ID Tokens are not encrypted but just Base64 encoded.
#### Flow
1. User wants to use app A to log in to app B.
2. App B redirects the user to app A's authorization server.
    - The request includes the client ID, redirect URI, response type, and scope set to "OPENID".
    - The "OPENID" scope lets app A know that this will be an OIDC exchange
3. App A's authorization server verifies who the user is, and if necessary prompts the user to log in (if the user has no active session with app A).
4. App A's authorization server asks for the user's consent.
5. App A's authorization server redirects the user to app B's specified redirect URI along with a temporary authorization code.
6. App B contacts app A's authorization server directly (not using the user's browser) with the client ID, client secret, and authorization code.
7. App A's authorization server verifies the information and then sends an ID token and an access token to app B.
8. App B should verify that the ID token was indeed sent by App A.
    - Verify the JWT signature, the `aud` claim, the `exp` claim, and the `iss` claim.
    - App B can also use the access token to send requests to app A's resource server for additional information.

## Authorization
### OAuth 2.0
> An authorization framework that defines authorization protocols and workflows. OAuth2.0 defines roles, authorization grants (or workflows), authorization requests and responses, and token handling. OpenID Connect (OIDC) protocols to verify user identity extends OAuth 2.0. | Auth0

- It is a security standard protocol for **authorization**. where one application gives access to it's data (Ex: user data) to another application with the use of tokens, which can be taken back.
- It enables an application to access specific resources and perform specific actions on behalf of a user.
#### How it Works
- An application must first acquire the client ID and client secret from an OAuth provider in order to connect to the OAuth provider.
  - They are required to be included in OAuth requests.
- Upon successful authorization, OAuth 2.0 returns an **Access Token**.
  - An access token grants permission to the specified resources.
  - Access tokens do not have a required format (can be in any string format) but they are often in JWT format.
  - The final recepient (`"aud"`) of an access token is the resource server.
#### Important Notes
- Access Tokens are NOT meant for authentication. They are strictly for authorization.
  - You cannot make any assumptions about the user’s identity using an Access Token.
- Access Tokens do NOT guarantee that a user is logged in.
#### Flow
1. User wants to allow app B to access his contacts stored in app A.
2. App B redirects the user to app A's authorization server.
    - The request includes the client ID, redirect URI, response type, and scope(s).
3. App A's authorization server verifies who the user is, and if necessary prompts the user to log in (if the user has no active session with app A).
4. App A's authorization server asks for the user's consent based on app B's requested scopes.
5. App A's authorization server redirects the user to app B's specified redirect URI along with a temporary authorization code.
6. App B contacts app A's authorization server directly (not using the user's browser) with the client ID, client secret, and authorization code.
7. App A's authorization server verifies the information and then sends an access token to app B.
    - The client does not "understand" the access token.
8. App B uses the access token to send requests to app A's resource server.
9. App A's resource server responds with the requested data.
#### Example
1. Request a user's GitHub identity.
    - `GET https://github.com/login/oauth/authorize`
    - Include parameters to configure the authorization.
      - `client_id`: the ID that GitHub issued when the application was registered to GitHub.
      - `scope`: represents the informations that the application wants from the user's GitHub identity.
2. GitHub redirects users back to the application/site.
    - `GET authorizationCallbackUrl`.
      - If the user accepted the authorization, GitHub redirects the user to the Authorization callback URL (redirect URI), which was registered to GitHub, along with a temporary authorization code (expires in 10 minutes) in the parameter (`req.query.code`).
    - `POST https://github.com/login/oauth/access_token`
      - Exchange the temporary authorization code for an access token.
      - Required Parameters:
        - `client_id`
        - `client_secret`
        - `code`
    - The Access Token is received from the response of the POST request.
    - *The Access Token only gives access to the informations mentioned in `scope`.*
3. Use the access token to access the API.
    - `GET https://api.github.com/user`
    - Required Headers:
      - `Authorization: "token OAUTH-TOKEN"`
        - OAUTH-TOKEN: the access token.
##### Food for Thought
- How should we tell whether the user is who they claim to be they are?
  - Scenario 1: User created an account using an email and password. User tries to login with GitHub OAuth.
    - The email used in GitHub is also the same email that was used to create the account. But the password registered for that email was not provided (since GitHub was used to login).
      - Allow login?
        - This means any other OAuth provider available with the same email will log the user in.
      - Tell the user to login using email and password?
        - Ex: "There is already an account by that email".
  - Scenario 2: User created an account using GitHub OAuth (no password saved in DB). User tries to login with email and password (which does not exist in DB).
    - Tell the user to login using GitHub (no password needs to be stored in the DB)?
      - Ex: "There is an account by that email but no password. Login with GitHub".

## Persistent Authentication
### Storing Session ID on Server and Client sends Cookies ([Session Management](#session-management))
- Use Http-Only cookies to avoid Cross-Site Scripting(XSS) attacks.
- But this is still vulnerable to Cross-Site Request Forgery(CSRF) attacks.
### Storing Session Information on Client
- Not very much concerned about CSRF attacks.
- But this is still vulnerable to XSS attacks, since an attacker may be able to see the user session information in the browser.

## Session Management
### Cookie and Session
#### [Cookie](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Client-side-Storage.md#cookies)
- **A Cookie is a small piece of data that a server creates and sends (in the response headers) to the client.**
- Once the browser receives a Cookie, the Cookie is automatically sent along with all subsequent requests to the server.
  - No need to write code for this, as it is the HTTP standard.
- *Cookies are stored on the browser.*
##### Properties
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
#### Session
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
##### `express-session` Session
- **The Session Store for the specific client can be accessed through the request object (`req.session`).**
- If the user provides valid login credentials, add information to the Session Store (update the state of the Session) in order to let the server know that the user has been verified and is logged in (therefore, send user-tailored content).
  - `req.session.isLoggedIn = true`, `req.session.user = user`.
  - In a controller.
- Through the use of `res.locals`, views can have access to the login information.
  - Sync `res.locals` with information on the Session object.
  - `res.locals.isLoggedIn = !!req.session.isLoggedIn`, `res.locals.loggedInUser = req.session.user`.
  - In a middleware.
##### `connect-mongo` Session Store
- A MongoDB based Session Store.
  - i.e. the sessions are stored on MongoDB and not on memory (default).
- Save the Session in a database so that users are logged in despite server restarts.
- Simply provide the MongoDB URL to create a Session Store.
  - The DB URL should not be revealed in the codebase.
#### Session Flow (default) Illustration
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
#### Issues to Consider
- You may not wish to create and save sessions for EVERY visitor.
  - This can be expensive for the DB.
  - It may be a better idea to do that for only users that are logged in (exclude anonymous users, bots, etc).
  - Ex: configure session to `resave: false` and `saveUninitialized: false`.
##### Session Flow Illustration
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/refImg/session-and-cookie-selective.png" alt="Session and Cookie Selective Flow Illustration" width="80%" />
</p>

- Only keep track of sessions for users that log in.

## Reference
[Stateless protocol - Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)  
[Authentication vs. Authorization - Auth0](https://auth0.com/docs/get-started/identity-fundamentals/authentication-and-authorization)  
[Simple and Secure API Authentication for SPAs | by Josh Krawczyk | Medium](https://medium.com/@sadnub/simple-and-secure-api-authentication-for-spas-e46bcea592ad)  
[OpenID Connect Protocol - Auth0](https://auth0.com/docs/authenticate/protocols/openid-connect-protocol)  
[ID Token and Access Token: What Is the Difference? - Auth0](https://auth0.com/blog/id-token-access-token-what-is-the-difference/)  
[Understanding OAuth: What Happens When You Log Into a Site with Google, Twitter, or Facebook](https://lifehacker.com/understanding-oauth-what-happens-when-you-log-into-a-s-5918086)  
[The Modern Guide to OAuth - FusionAuth](https://fusionauth.io/learn/expert-advice/oauth/modern-guide-to-oauth)  
[Authorizing OAuth Apps - GitHub Docs](https://docs.github.com/en/developers/apps/building-oauth-apps/authorizing-oauth-apps)  
[Session vs Token Authentication in 100s - YouTube](https://www.youtube.com/watch?v=UBUNrFtufWo&ab_channel=Fireship)  
[Session cookies concepts - IBM](https://www.ibm.com/docs/en/sva/9.0?topic=cookies-session-concepts)  
