# Terminologies

## Static Web Page vs. Dynamic Web Page
### Static Web Page
- Web pages that are fixed and display the same content for every user (ex: HTML and CSS).
- Static websites usually come with a fixed number of web pages that have a specific layout.
- The page's source code won't change.
- Informational.
### Dynamic Web Page
- Web page that displays different content and provide user interaction (use advanced programming and databases in addition to HTML).
- *Content of the web page is generated and displayed based on what actions the users make on the page.*
- When a user accesses a dynamic website, the site can be changed through code that is run in the browser and/or on the server.
  - The end result is the same as that on a static website: an HTML page displayed on the browser.
- Combination of client-side and server-side scripting.
#### Client-side Scripting
- Code that is executed by the browser (JS).
#### Server-side Scripting
- Code that is executed by the server (before the content is sent to the user's browser).
- Makes the HTML and CSS functional.
### Reference
[Static vs Dynamic Website: What Is the Difference?](https://wpamelia.com/static-vs-dynamic-website/#:~:text=Static%20websites%20are%20ones%20that,databases%20in%20addition%20to%20HTML.)

## Single Page Application (SPA) vs. Multi Page Application (MPA)
### SPA
> A web app implementation that loads only a single web document, and then updates the body content of that single document via JS APIs such as `XMLHttpRequest` and `Fetch` when different content is to be shown.
> Allows users to use websites without loading whole new pages from the server, which can result in performance gains and a more dynamix experience, with some tradeoff disadvantages such as SEO, more effort required to maintain state, implement navigation, and do meaningful performance monitoring.
- SPA frameworks: React, Angular, Vue.JS.
#### Pros
- SPA is fast because most resources (HTML, CSS, Scripts) are only loaded once, and only data is transitted back and forth.
- No need to write code to render pages on the server. Easier to get started on a project without having to use any server.
- Feasible debugging, as network operations, page elements, and associated data can be monitored and investigated on browser.
- Can cache any local storage effectively. An application sends only one request, store all the data, then it can use this data and works even offline.
#### Cons
- SEO optimization.
- Slow to download because heavy client frameworks are required to be loaded to the client.
- JS needs to be present and enabled in client's browser.
- Less secure due to Cross-Site Scripting (XSS), which enables attackers to inject client-side scripts into the web app by other users.
- Memory leak in JS can cause system to slow down.
### MPA
- "Traditional" way.
- Every change requests rendering a new page from the server in the browser.
- Larger than SPAs.
#### Pros
- Good for users who need a visual map to navigate the application.
- Good and easier SEO management. It gives better chances to rank for different keywords since an application can be optimized for one keyword per page.
#### Cons
- Frontend and backend development are tightly coupled.
- Development becomes quite complex. Need to use frameworks for either client and server side, requiring longer development time.
### Hybrid Site
- SPA that uses URL anchors as synthetic pages enabling more in browser navigation and preference functionality.
### Reference
[SPA (Single-page application)](https://developer.mozilla.org/en-US/docs/Glossary/SPA)  


## Client-Side Rendering (CSR) vs. Server-Side Rending (SSR)
### CSR
> Allows the website to be updated in the browser almost instantly when navigating to different pages, but requires more of an initial download hit and extra rendering on the client at the beginning. The website is slower on an initial visit, but can be faster to navigate.
- Website is rendered on the client-side.
- When the client sends a request to the server, the server will send a single page, will be the skeleton of the page to the client, along with the JS file. The JS file will turn the page into a fully rendered page. The content/data of the page comes from the API. The client makes a request to the API, which then renders the data onto the page. If the client navigates to a different route, instead of the server sending the page again, the client will re-render the page according to the route that the client requested. Hence, the page that is used is always the same page as the first request.
#### Use If:
- SEO is not priority.
- Website has many interactions.
- Building a web application.
### SSR
> Website is rendered on the server, so it offers quicker first load, but navigating between pages requires downloading new HTML content. It works great across browsers, but it suffers in terms of time navigating between pages and therefore general perceived performance - loading a page requires a new round trip to the server.
- Website arrives on the client-side fully-rendered.
- When the client sends a request to the server, the server compiles everything and renders it (including the requested data) into a fully rendered page, and then send it to the client as a response.
- For every different route that the client navigates to, the server will repeat the steps again.
#### Use If:
- SEO is priority.
- Need faster initial loading.
- Content of website doesn't need much user interaction.
### Reference
[Progressive web app structure | MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)

## Library vs. Framework
### Library
- When using a library, we are in charge. We control the flow of the application code and decide when to use the library.
- Libraries often do smaller things or are single-purpose.
- Ex: AXIOS (making http requests), color libraries, ASCII art libraries.
### Framework
- When using a framework, the framework is in charge. The framework provides the structure for an application and we incorporate our code according to how the framework works.
- Trade-off between control, and speed of development and features.
- Frameworks are geared into helping us make full applications.
- Ex: Express, Ruby on Rails.
