# Client-side Storage

## Table of Contents
- [Cookies](#cookies)
  - [Types of Cookies](#types-of-cookies)
  - [3 Main Purposes](#3-main-purposes)
  - [`document.cookie` Object](#document.cookie-object)
  - [Cookie HTTP Header](#cookie-http-header)
  - [Restricting Access to Cookies](#restricting-access-to-cookies)
  - [Defining Where Cookies are Sent](#defining-where-cookies-are-sent)
- [Web Storage API](#web-storage-api)
  - [Local Storage](#local-storage)
  - [Session Storage](#session-storage)
- [Cookies vs Local Storage vs Session Storage](#cookies-vs-local-storage-vs-session-storage)
- [Reference](#reference)

## Cookies
- a.k.a. HTTP cookie, magic cookie, web cookie, internet/browser cookie.
> ... small blocks of data created by a web server while a user is browsing a website and placed on the user's computer or other device by the user's web browser. | WikiPedia

> The browser may store it and send it back with later requests to the same server. Typically, it’s used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the stateless HTTP protocol. | MDN

- Cookies are stored in a file located inside the web browser.
- Cookies are sent along with every request to the server, hence can worsen performance.
  - Therefore, it may be better to use modern storage APIs such as the Web Storage API (localStorage, sessionStorage) or IndexedDB.
  - If there is no need to send the information to the server, use Web Storage API.
### Types of Cookies
#### Session Cookies
- Expires when the current session ends.
- Used only when a user is actively navigating a website. Once the user leaves the website, the session cookie disappears.
#### Permanent Cookies
- Expires at a specified date (`Expires` or `Max-Age` attribute).
### 3 Main Purposes
#### 1. Session Management
- For anything that the server should remember.
- Ex: login/authentication.
  - Track whether a user is logged in, and if so, under what name.
- Ex: shopping cart.
  - Without cookies, the cart would reset to zero every time a new link is clicked on the site.
- Ex: imdb recently viewed.
#### 2. Personalization
- User preferences, themes, and other settings.
- Ex: default to dark mode.
#### 3. Tracking
- Recording and analyzing user behavior.
- Can be used to create long-term records of multiple visits to the same site.
### `document.cookie` Object
- The only way to interact (see and set) with cookies).
- Has access to name, value, domain, path, expiration date, etc.
#### Creating Cookies
- `document.cookie = ""`
  - Creates a new cookie with the provided information.
  - *Cookies created in JavaScript cannot include the `HttpOnly` flag*
- Example
  ```js
  // Cookie 1
  document.cookie = "name=tom; expires=" + new Date(2022, 0, 1).toUTCString();
  
  // Cookie 2
  document.cookie = "name=jerry; expires=" + new Date(9999, 0, 1).toUTCString();
  ```
### Cookie HTTP Header
> The `Cookie` HTTP request header contains stored HTTP cookies associated with the server (i.e. previously sent by the server with the `Set-Cookie` header or set in Javascript using `Document.cookie`). | MDN

> The `Cookie` header is optional and may be omitted if, for example, the browser’s privacy settings block cookies. | MDN
#### Flow Example
- The server uses the `Set-Cookie` HTTP response header to send cookies to the client.
  - Ex: `Set-Cookie: name=tom`
- Then, with every subsequent request to the server, the browser sends back all previously stored cookies to the server using the `Cookie` header.
  - Ex: `Cookie: name=tom`
### Restricting Access to Cookies
#### `Secure` Attribute
- Sent to the server only with an encrypted request over the HTTPZS protocol, never with unsecured HTTP (except on localhost).
  - HTTP sites cannot set cookies with the `Secure` attribute.
- Helps defend against man-in-the-middle attackers.
#### `HttpOnly` Attribute
- `HttpOnly` cookies are not accessible by JavaScript (cannot access using `document.cookie`).
  - So we cannot read it and include it in the request header.
  - So in the client-side, to check for JWT, we need to check the cookies and not the header.
- `HttpOnly` cookies are only sent to the server.
- Helps defend against XSS attacks.
- However, we are still prone to CSRF.
#### Example
- Server-Side
  - `Set-Cookie: id=tom; Expires=Mon, 11 Nov 2021 08:00:00 GMT; Secure; HttpOnly`
### Defining Where Cookies are Sent
#### `Domain` Attribute
- Specifies which hosts are allowed to receive the cookie.
- Defaults to the same host that set the cookie, excluding subdomains.
- Specifying a domain always includes subdomains.
#### `Path` Attribute
- Indicates a URL path that must exist in the requested URL in order to send the `Cookie` header.
- Ex: if `Path=/docs` is set, `/docs/Web/`, `/docs/Web/HTTP/` all match.

## Web Storage API
### Local Storage
- Holds a separate storage area for each domain, even if the browser is closed and reopened.
- Stored data do not have an expiration date.
- All stored data are converted to strings.
#### Syntax
- `localStorage.setItem("keyName", "value")`
  - Save a data item in storage, in the form of a key-value pair.
  - Ex: `localStorage.setItem("name", "Jerry")`
- `localStorage.getItem("keyName")`
  - Get the corresponding value of the provided key.
  - Ex: `localStorage.getItem("name")` // returns "Jerry"
- `localStorage.removeItem("keyName")`
  - Removes the corresponding key-value pair of the provided key.
  - Ex: `localStorage.removeItem("name")`
- `localStorage.clear()`
  - Remove ALL the items in local storage.
### Session Storage
- Holds a separate storage area for each domain for until the druation of the page.
  - *i.e. once the browser is closed, the stored data is gone.*

## Cookies vs Local Storage vs Session Storage
|                    | Cookies            | Local Storage | Session Storage |
| ------------------ | ------------------ | ------------- | --------------- |
| Capacity           | 4kb                | 10mb          | 5mb             |
| Browsers           | HTML4/HTML5        | HTML5         | HTML5           |
| Accessible from    | Any window         | Any window    | Same tab        |
| Expires            | Manually set       | Never         | On tab close    |
| Storage Location   | Browser and Server | Browser only  | Browser only    |
| Sent with Requests | Yes                | No            | No              |

## Reference
[HTTP cookie - Wikipedia](https://en.wikipedia.org/wiki/HTTP_cookie)  
[Using HTTP cookies - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)  
[JavaScript Cookies vs Local Storage vs Session - YouTube](https://www.youtube.com/watch?v=GihQAC1I39Q&ab_channel=WebDevSimplified)  
[Client-side storage - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage)  
