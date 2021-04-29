# Basics

## HTTP vs HTTPS
### Hypertext Transfer Protocol (HTTP)
- Allows for the communication between different systems (most commonly used to transfer data from a web server to a browser to allow users to view web pages).
- Information that flows from server to browser is not encrypted, which means it can easily be stolen.
### Hypertext Transfer Protocol Secure (HTTPS)
- Uses a **Secure Sockets Layer (SSL) certificate** or **Transport Layer Security (TLS) certificate**, which is an upgraded version of SSL, to create a secure encrypted connection between the server and the browser, thereby protecting potentially sensitive information from being stolen as it is transferred between the server and the browser.
- Crucial especially for websites requiring sensitive data (ex: credit card info, password).
- Can also boost SEO efforts.
- It is required for Accelerated Mobile Pages (AMP).
  - AMP: a way to load content onto mobile devices at a much faster rate.
### Mixed Content
> An HTTPS page that includes content fetched using cleartext HTTP is called a **mixed content page**. Pages like this are only partially encrypted, leaving the unencrypted content accessible to sniffers and man-in-the-middle attackers. That leaves the pages unsafe.
- Fix
  - Serve all the content as HTTPS instead of HTTP (http:// --> https://).
### Reference
[HTTP vs HTTPS](https://seopressor.com/blog/http-vs-https/)  
[Mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)  
[How to fix a website with blocked mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content/How_to_fix_website_with_mixed_content)

## Cookies
- Short for magic cookie.
  > A packet of data that a computer receives and then sends back without changing or altering it.
  - a.k.a. HTTP cookie, web cookie, internet/browser cookie.
- Upon visiting a website, the website sends the cookie to the computer. Then the cookie is stored in a file located inside the web browser.
- Purpose
  - To help the website keep track of your visits and activity.
  - Ex: online retailers use cookies to keep track of the items in a user's shopping cart as they explore the site. Without cookies, the cart would reset to zero every time a new link is clicked on the site.
  - Ex: imdb recently viewed.
  - Ex: record login information.
- Types of Cookies
  - Session Cookies: used only when a user is actively navigating a website. Once the user leaves the site, the session cookie disappears.
  - Tracking Cookies: can be used to create long-term records of multiple visits to the same site.
  - Authentication Cookies: track whether a user is logged in, and if so, under what name.
- Note
  - The data in a cookie doesn't change when it travels back and forth, hence safe from virus/malware.

### Reference
[What are computer cookies?](https://us.norton.com/internetsecurity-privacy-what-are-cookies.html)
