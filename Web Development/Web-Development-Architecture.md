# Web Development Architecture

## Table of Contents
- [JAMStack](#jamstack)

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
### JAM
- Client-side **JavaScript**, reusable **APIs**, prebuilt **Markup**.
#### JavaScript
- All dynamic actions are executed by JavaScript (JS, React, Vue, etc.) running completely on the client.
#### API
- All server-side processes or database actions are abstracted into reusable APIs, accessed over HTTPS with JavaScript.
- These APIs can be custom-built or third-party services.
#### Markup
- Templated markup should be prebuilt at deploy time, usually using a site generator for content sites, or a build tool for web apps.
### Why JAMStack?
- Traditional websites or CMSs (Ex: WordPress) rely heavily on servers, plugins, and databases.
#### Fast
- JAMStack is fast because files are pre-built and are served over a CDN.
- HTML is already generated during deploy time (it is ready to go before the client even requests it) and immediately served via CDN without any interference or backend delays.
#### Secure
- Since everything works over APIs, there are no database or security breaches.
#### Cheaper and Easire to Scale
- JAMStack sites contain just a few files of minimal size that can be served anywhere. To scale, simply serve the files else where or via CDNs.

### Reference
[Glossary of Jamstack | Jamstack](https://jamstack.org/glossary/)  
[An introduction to the JAMstack: the architecture of the modern web](https://www.freecodecamp.org/news/an-introduction-to-the-jamstack-the-architecture-of-the-modern-web-c4a0d128d9ca/)  
