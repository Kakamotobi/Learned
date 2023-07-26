# Auth

## Table of Contents
- [HTTP is Stateless](#http-is-stateless)
- [Authentication vs. Authorization](#authentication-vs-authorization)
  - [Terminology](#terminology)
- [Authentication](#authentication)
  - [Session Management / Persistent Authentication](#session-management--persistent-authentication)
    - [Cookie-based (Session Cookie) Authentication](#cookie-based-session-cookie-authentication)
    - [Token-based Authentication](#token-based-authentication)
  - [OpenID Connect(OIDC) Protocol](#openid-connectoidc-protocol)
- [Authorization](#authorization)
  - [OAuth 2.0](#oauth-20)
  - [Proof Key for Code Exchange(PKCE) with OAuth 2.0](#proof-key-for-code-exchangepkce-with-oauth-20)

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
- **OP:** OAuth 2.0 or OpenID Connect(OIDC) Provider.
  - **Authorization Server:** the OP's server that facilitates the authorization/authentication.
  - **Resource Server:** the server that holds the resource that a RP wishes to access.
  - Sometimes the Authorization Server and Resource Server can be the same.
- **RP:** Relying Party (a.k.a. the OAuth/OIDC client, third-party application).
  - The application that wants to access the Resource Owner's data and perhaps perform an action on behalf of the Resource Owner.
  - **Client ID:** the ID that is used to identify the RP with the Authorization Server.
  - **Client Secret:** the secret password that only the RP and the Authorization Server know.
- **Resource Owner:** the user to whom the protected resource belongs to.
- **Consent:** the Authorization Server verifies with the Resource Owner whether or not they want to give the RP permission.
- **Redirect URI:** the URL that the Authorization Server will redirect the Resource Owner back to, after granting permission to the client.
  - a.k.a. callback URL.
- **Authorization Code:** a temporary code that the RP can exchange for some tokens (Access, Refresh, ID Token, depending on the protocol) from the OP.
- **Access Token:** a token/string that the RP can use to make requests to the OP's resource server.
- **Refresh Token:** a token/string that the RP can use to get a new Access Token without the need for user interaction.
- **ID Token:** a JWT signed by the OP, which contains "claims" about the user's authentication and some basic user information (Ex: name, email).
  - It is the priamry extension that OIDC makes to OAuth 2.0.
- **Scope:** the permissions that the RP wants.
- **Session:** a continuous period of time during which a user access a RP.

## Authentication
### [Session Management / Persistent Authentication](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Auth/Session-Management.md)
#### Cookie-based (Session Cookie) Authentication
- This is the traditional approach on the web for session management.
- **The authentication state (sessions) is handled on the server.**
##### Flow
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
##### Notes
- Use Http-Only cookies to avoid Cross-Site Scripting(XSS) attacks.
- This approach can be vulnerable to Cross-Site Request Forgery (CSRF).
- The session ID must be stored in a database or keep it in memory on the server, which can be a bottleneck in production since most cloud applications are scaled horizontally.
#### Token-based Authentication
- Use of encrypted security tokens to verify a user's identity.
- **The authentication state (JWT) is handled on the client.**
##### Token Storage on Client ft. Security
###### Encrypted Http-Only Cookie
- An Http-Only Cookie is a cookie that prevents the frontend code from reading or writing to the cookie header, effectively blocking access to the contents of the cookie (not vulnerable to cross-site scripting(XSS) attacks).
- Upon first authentication, return the cookie instead of the tokens.
- You can set the lifetime on these cookies to limit the logged in state.
- Simply log a user out by deleting the cookie.
- Encrypt the cookie with a key that is stored in the server just in case the user's computer is compromised.
- **Cookie attributes to use:**
  - `httpOnly`, `secure`, `SameSite=strict`.
- **CSRF Protection**
  - Use a CSRF token and API key to encode the JWT to ensure that the requests are actually coming from your application.
##### Flow
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
##### Notes
- Not 
- Tokens can be hijacked by attackers and it can be difficult to invalidate hijacked tokens.
### OpenID Connect(OIDC) Protocol
> OpenID Connect (OIDC) is an identity layer built on top of the OAuth 2.0 framework. It allows third-party applications to verify the identity of the end-user and to obtain basic user profile information. | Auth0

- OIDC cannot work without the underlying OAuth 2.0 framework.
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
#### Illustration Example
<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/Auth/refImg/OpenIdConnect.jpg" alt="OpenId Connect" width="80%"/>
</div>

## Authorization
### OAuth 2.0
> An authorization framework that defines authorization protocols and workflows. OAuth2.0 defines roles, authorization grants (or workflows), authorization requests and responses, and token handling. OpenID Connect (OIDC) protocols to verify user identity extends OAuth 2.0. | Auth0

- OAuth 2.0 is an **authorization** protocol where a user allows an application to grant a third-party application access to their protected resources, enabling the application to perform specific actions on behalf of the user, without having to reveal their credentials to the third-party application.
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
##### Using OAuth 2.0 for Authentication
- Technically, OAuth 2.0 was designed for authorization only. It was not intended for authentication. However, many applications use OAuth 2.0 to authenticate users because the OP giving an Access Token implies that the user is who they claim to be.
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
#### Illustration Example
<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/Auth/refImg/OAuth2.0.jpg" alt="OAuth2.0" width="80%"/>
</div>

#### GitHub OAuth Example
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
### Proof Key for Code Exchange(PKCE) with OAuth 2.0
> PKCE (RFC 7636) is an extension to the Authorization Code flow to prevent CSRF and authorization code injection attacks. | oauth.net

#### How it Works
- The application generates a random value (called **Code Verifier**) at the beginning of the authorization flow.
  - The Code Verifier is hashed, which is called the **Code Challenge**.
- The application starts the Authorization Code flow as normal, but with the exception of including the Code Challenge in the query string for the request to the Authorization Server.
  - The Authorization Server receives and stores the Code Challenge (to be used for verification later), and upon successfully authenticating the user, redirects back to the application along with an Authorization Code.
  - The application requests to exchange the Authorization Code for an Access Token, sending the Code Verifier along with it instead of a fixed secret.
  - The Authorization Server hashes the Code Verifier and compares it to the previously stored Code Challenge.
  - If the hashed values match, The Authorization Server returns an Access Token to the application.

<p align="center">
  <img src="https://developer.okta.com/assets-jekyll/blog/okta-authjs-pkce/pkce-59cd81484ee5be4248d4f8efc986070d7d6ac20b8091da3b8377bf1e278a0b54.svg" alt="PKCE" width="80%" /><br>
</p>

## Reference
[Stateless protocol - Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)  
[Authentication vs. Authorization - Auth0](https://auth0.com/docs/get-started/identity-fundamentals/authentication-and-authorization)  
[Simple and Secure API Authentication for SPAs | by Josh Krawczyk | Medium](https://medium.com/@sadnub/simple-and-secure-api-authentication-for-spas-e46bcea592ad)  
[OpenID Connect Protocol - Auth0](https://auth0.com/docs/authenticate/protocols/openid-connect-protocol)  
[What is OAuth 2.0 and what does it do for you? - Auth0](https://auth0.com/intro-to-iam/what-is-oauth-2/)  
[ID Token and Access Token: What Is the Difference? - Auth0](https://auth0.com/blog/id-token-access-token-what-is-the-difference/)  
[Understanding OAuth: What Happens When You Log Into a Site with Google, Twitter, or Facebook](https://lifehacker.com/understanding-oauth-what-happens-when-you-log-into-a-s-5918086)  
[The Modern Guide to OAuth - FusionAuth](https://fusionauth.io/learn/expert-advice/oauth/modern-guide-to-oauth)  
[Authorizing OAuth Apps - GitHub Docs](https://docs.github.com/en/developers/apps/building-oauth-apps/authorizing-oauth-apps)  
[Session vs Token Authentication in 100s - YouTube](https://www.youtube.com/watch?v=UBUNrFtufWo&ab_channel=Fireship)  
[security - Where to store JWT in browser? How to protect against CSRF? - StackOverflow](https://stackoverflow.com/questions/27067251/where-to-store-jwt-in-browser-how-to-protect-against-csrf)  
[PKCE for OAuth 2.0 - oauth.net](https://oauth.net/2/pkce/)  
[Implement the OAuth 2.0 Authorization Code with PKCE Flow | Okta Developer](https://developer.okta.com/blog/2019/08/22/okta-authjs-pkce)  
