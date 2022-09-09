# Security

## Table of Contents
- [Cross-Site Request Forgery (CSRF)](#cross-site-request-forgery-csrf)
  - [CSRF Mitigations](#csrf-mitigations)

## Cross-Site Request Forgery (CSRF/XSRF)
### What is CSRF?
- **CSRF refers to when a domain forges a request to another domain in order to modify some value (by taking advantage of browsers' functionality of automatic cookie transmissions to any domain that you have an active cookie for).**
  - Ex: evil.com making a request to the "delete account" endpoint on google.com.
- The attacker is able to request state changes but is not able to see the response data.
  - Ex: change password, delete your account, etc. But they cannot see the response of doing so.
  - However, this does not remain true if the website is also vulnerable to XSS.
- A CSRF attack looks very authentic and looks like it originated from the user since it has the user's IP address, cookie, etc.
#### Background
- We want users to be able to stay logged in (session persistence). This could be achieved through the use of cookies containing session information.
- If a malicious website somehow gets hold of a user's authentication information (whether it was stored in a cookie or token), it can make a request just like the real authenticated user can.
  - For example, if `evil.com` gets hold of your cookie that contains your session ID, it can use it to make the same request, cross-domain, as thought it was you making a request in `website.com` as the actual user.
- So, how can the server receiving the request be sure of where the request came from?
#### Cookie Flow
- ***On every single request to a server, cookies are automatically sent regardless of the domain that is sending the request.***
  - Ex: google.com and evil.com sends the same cookies to the google server. 
#### Cross Domain Access Controls
##### Same Origin Policy (SOP)
- **The Same Origin Policy (SOP) restricts websites with different origins accessing other websites.**
  - Therefore, only websites of the same origin (schema, domain, and port) will be able to interact.
  - This applies to iframes, AJAX requests, etc.
- ***Under SOP, a malicious website can make requests but they cannot read the responses (blocked by SOP).***
- SOP is very important since cookies are automatically sent on every request to that domain.
##### Cross Origin Resource Sharing (CORS)
- **CORS is a mechanism that allows restricted resources on a web page to be requested from a different domain from which the resource was served.**
- A server can indiciate any origins other than its own from which a browser should allow loading resources.
  - i.e. a bypass for SOP.
- For CORS to work, the server has to explicitly state it in its response headers.
  - `Access-Control-Allow-Origin: your-website` - indicating permission for your website to access the resources.
  - `Access-Control-Allow-Headers: Content-Type` - indicating permission for your website to set the content type of the response.
- It is useful since APIs are usually hosted on a different domain from the one that is making requests. Therefore, it is necessary to have a proper way to communicate between different domains.
### CSRF Mitigations
#### Re-Authentication
- "Prove to me again that you are actually who you say you are."
  - Ex: through captcha, password, etc.
- This can be very annoying to have to provide credentials again even after logging in. However, it may be useful for situations where the request being made is important or "big" (Ex: delete account).
#### Unique Form Tokens on Every POST, PUT, and DELETE Request
- Include the token in both the cookie header and body of the message, so that it is sent on every POST, PUT, and DELETE request.
#### Use Anti-CSRF Tokens
- The web application's server generates a random token and sends to the frontend. And on every request, the token is validated to ensure that it is not a malicious website making the request (since the malicious website does not know about the Anti-CSRF token).
#### Libraries
- Use libraries that include built-in CSRF protection.

## Reference
[Cross-Site Request Forgery (CSRF) Explained - YouTube](https://www.youtube.com/watch?v=eWEgUcHPle0&ab_channel=PwnFunction)  
[CSRF Tutorial - A Guide to Better Understand and Defend Against Cross-Site Request Forgery (CSRF) - YouTube](https://www.youtube.com/watch?v=13QPmRuhbhU&ab_channel=FullstackAcademy)  
