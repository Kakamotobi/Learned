# Web Development Architecture
Architectures and design patterns for developing web applications.

## Table of Contents
- [Single-Page Application (SPA) vs. Multi-Page Application (MPA)](#single-page-application-spa-vs-multi-page-application-mpa)
- [JAMStack](#jamstack)

## Single-Page Application (SPA) vs. Multi-Page Application (MPA)
- Concerned with how many pages are used.
### SPA
> A web app implementation that loads only a single web document, and then updates the body content of that single document via JS APIs such as `XMLHttpRequest` and `Fetch` when different content is to be shown.
> Allows users to use websites without loading whole new pages from the server, which can result in performance gains and a more dynamix experience, with some tradeoff disadvantages such as SEO, more effort required to maintain state, implement navigation, and do meaningful performance monitoring. | MDN
- SPA frameworks: React, Angular, Vue.JS.
- Ex: Google Maps
#### SPA with SSR (and CSR)
- SPAs that are rendered on the server-side are the isomorphic JavaScript (implying the use of Node.js).
- The first rendering of the SPA is done from the server instead of the browser. Following renders are done in the browser.
- Node.js is used to generate the initial HTML. In the browser, the same JS code is executed (hydration process: event listeners are attached to the HTML).
- React, Vue, Angular are capable of executing in both Node.js and the browser. But frameworks such as Next.js and Nuxt.js exist for better developing experience.
#### SPA with CSR
- SPAs that are rendered on the client-side are applications built with frontend libraries/frameworks with a client-side router, which allows navigating the website without having to load a new page.
- Basically "original" React.
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
- An application with two or more pages.
- Every change requests rendering a new page from the server in the browser.
- "Traditional" way.
- Larger than SPAs.
#### MPA with SSR
- MPAs that are rendered on the server-side are full-stack applications built with server-side frameworks and template engines.
- The pages of the website are rendered on the fly by the server.
#### MPA with CSR
- MPAs that are rendered on the client-side are regular static sites.
- The pages are built with just HTML, CSS, and JS.
- Can be deployed without a server and dynamic content is provided by JS and APIs (JAMStack).
- Does not utilize a client-side router.
#### Pros
- Good for users who need a visual map to navigate the application.
- Good and easier SEO management. It gives better chances to rank for different keywords since an application can be optimized for one keyword per page.
#### Cons
- Frontend and backend development are tightly coupled.
- Development becomes quite complex. Need to use frameworks for either client and server side, requiring longer development time.
### Hybrid Site
- SPA that uses URL anchors as synthetic pages enabling more in browser navigation and preference functionality.
### Reference
[SPA (Single-page application) | MDN](https://developer.mozilla.org/en-US/docs/Glossary/SPA)  
[Understanding Rendering in the Web Apps: SPA vs MPA - DEV Community](https://dev.to/snickdx/understanding-rendering-in-web-apps-spa-vs-mpa-49ef#:~:text=Server%2DSide%20Rendered%20Multi%2DPage,also%20fall%20into%20this%20category.)  

## JAMStack
> A way of thinking about how to build for the web. The UI is compiled, the frontend is decoupled, and data is pulled in as needed. | Jamstack
  - "UI is compiled" (a.k.a. pre-rendered/pre-generated)
    > To generate the markup which represents a view in advance of when it is required. This happens during a build rather than on-demand so that web servers do not need to perform this activity for each request received. | Jamstack
  - "frontend is decoupled"
    > Decoupling is the process of creating a clean separation between systems or services. By decoupling the services needed to operate a site, each component part can become easier to reason about, can be independently swapped out or upgraded, and can be designated the purview of dedicated specialists either within an organization, or as a third party. | Jamstack

> With Jamstack, the entire front end is prebuilt into highly optimized static pages and assets during a build process. This process of pre-rendering results in sites which can be served directly from a CDN, reducing the cost, complexity and risk, of dynamic servers as critical infrastructure. | Jamstack

> JAMstack is a modern web development architecture. It is not a programming language or any form of tool. It is more of a web development practice aimed towards enforcing better performance, higher security, lower cost of scaling, and better developer experience. | freeCodeCamp

- JAMStack is not a technology. It is a design pattern that is implemented in technology.
- The markup and UI assets are served directly from a CDN, which makes the delivery very quick and secure.
- JAMStack projects can be distributed (for example, through hosting platforms like Vercel) instead of relying on a server.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/Jamstack.png" alt="Jamstack Structure" width="80%" />
</p>

### JAM
- Client-side **JavaScript**, reusable **APIs**, prebuilt **Markup**.
- Pre-rendered content is served to a CDN and made dynamic through APIs and serverless functions.
#### JavaScript
- All dynamic actions are executed by JavaScript (JS, React, Vue, etc.) running completely on the client.
#### API
- All server-side processes or database actions are abstracted into reusable APIs, accessed over HTTPS with JavaScript.
- These APIs can be custom-built or third-party services.
#### Markup
- Templated markup should be prebuilt at deploy time, usually using a site generator (Ex: Next.js & Gatsby for React) for content sites, or a build tool (Ex: Webpack) for web apps.
### Why JAMStack?
- Traditional websites or CMSs (Ex: WordPress) rely heavily on servers, plugins, and databases.
#### Fast
- JAMStack is fast because files are pre-built and are served over a CDN.
- HTML is already generated during deploy time (it is ready to go before the client even requests it) and immediately served via CDN without any interference or backend delays.
#### Secure
- Since everything works over APIs, there are no database or security breaches.
#### Cheaper and Easier to Scale
- JAMStack sites contain just a few files of minimal size that can be served anywhere. To scale, simply serve the files else where or via CDNs.
### JAMStack vs SPA
- SPA builds the application within the client's browser.
  - When a client requests a page, the markup and JS are sent to the browser and the page is built within the browser (built at run-time).
- JAMStack buidls the application at build-time.
### Reference
[Glossary of Jamstack | Jamstack](https://jamstack.org/glossary/)  
[An introduction to the JAMstack: the architecture of the modern web](https://www.freecodecamp.org/news/an-introduction-to-the-jamstack-the-architecture-of-the-modern-web-c4a0d128d9ca/)  

## Reference
[An Ultimate Guide of Web Application Architecture](https://www.simform.com/blog/web-application-architecture/#section6)  
