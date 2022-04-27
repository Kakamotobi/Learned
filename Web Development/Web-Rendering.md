# Web Rendering

## Table of Contents
- [Client-Side Rendering (CSR)](#client-side-rendering-csr)
- [Server-Side Rendering (SSR)](#server-side-rendering-ssr)
- [Static-Site Generation (SSG)](#static-site-generation-ssg)
- [Incremental Static Regeneration (ISR)](#incremental-static-regeneration-isr)

## Client-Side Rendering (CSR)
> Allows the website to be updated in the browser almost instantly when navigating to different pages, but requires more of an initial download hit and extra rendering on the client at the beginning. The website is slower on an initial visit, but can be faster to navigate.
- Website is rendered on the client-side.
- *When the client sends a request to the server, the server will send a single HTML index page, which will be the skeleton of the page to the client, followed by the JS file. The JS file, which is large as it contains the logic and the source codes of the framework/library, will turn the page into a fully rendered page. If additional content/data is needed, the client makes a request to the API, which then renders the data (JSON) onto the page. If the client navigates to a different route, instead of the server sending the page again, the client will re-render the page according to the route that the client requested. Hence, the page that is used is always the same page as the first request.*
### Illustration
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/CSR.png" alt="Client-Side Rendering" width="80%" />
</p>

1. Client requests page.
2. Server sends an almost empty `index.html` file. // Users see a blank screen.
3. Client requests the JS files linked in the `index.html` file.
4. JS file containing web application logic (libraries, frameworks) that creates dynamic HTML is received from the server. // TTV, TTI
    - **Users can now see and interact with the site.**
### Use If:
- Website has many user interactions.
- Building a web application.
- SEO is not a priority.
- Ex: login pages, dashboards.
### Cons
- Low SEO.
  - Web crawlers check the index.html file for titles, etc. but HTML files for CSR are mostly empty, making it difficult for SEO.
- Slow initial loading
### Question to Think About
- How to effectively divide and prioritize essential content to be sent first in order to quicken TTV and TTI.

## Server-Side Rendering (SSR)
> Website is rendered on the server, so it offers quicker first load, but navigating between pages requires downloading new HTML content. It works great across browsers, but it suffers in terms of time navigating between pages and therefore general perceived performance - loading a page requires a new round trip to the server.
- Website arrives on the client-side fully-rendered.
- *When the client sends a request to the server, the server compiles everything and renders it (including the requested data) into a fully rendered HTML page, and then send it to the client as a response along with JS source code.*
- For every different route that the client navigates to, the server will repeat the steps again.
- On every request (incl. refresh), the data has to be fetched again as well.
#### Illustration
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/SSR.png" alt="Server-Side Rendering" width="80%" />
</p>

1. Client requests page.
2. Server sends an already made `index.html` file. // TTV
    - **Users can see the site.**
3. Client requests the JS file linked in the html file.
4. JS file is received from the server. // TTI
    - **Users can now interact with the site.**
#### Use If:
- SEO is priority.
- Need faster initial loading.
- Website doesn't have many user interactions.
- Ex: reddit.
#### Cons
- Blinking issue
  - Every click results to bringing the website from the server.
- Server-side overhead.
- Users may need to wait before interacting.
  - Sometimes result to delayed response when clicking on links, etc.
#### Question to Think About
- How to reduce the time between TTV and TTI.

## Static-Site Generation (SSG)
> A static site generator is a tool that generates a full static HTML website based on raw data and a set of templates. Essentially, a static site generator automates the task of coding individual HTML pages and gets those pages ready to serve to users ahead of time. Because these HTML pages are pre-built, they can load very quickly in users' browsers.
- Pre-generate the website statically and deploy it onto the server.
  - i.e., on every rebuild, the data is fetched and placed in the markup statically, which is then deployed.
  - Therefore, if the client requests the page, there is no need to fetch data.
- However, this does not mean that the website has to be static. JS allows the application of dynamic executions (Ex: request data).
- *SSG is usually considered to be the fastest way to serve a website because the heavylifting of the markup and putting the data into the markup is done before the client even asks for the page.*
- SSG Ex: Gatsby.js, Next.js (used to be SSR only).
#### Illustration
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/SSG.png" alt="Static Site Generator" width="80%" />
</p>

1. Client requests page.
2. The server sends all the HTML files that the website uses with the data (text, images, etc.) prebuilt/generated. // TTV, TTI
    - For example, there will not be more fetching data from the API after the client request because that was already done when uploading the website onto vercel.
    - Therefore, when a user visits a website, the page loads almost instantly.
#### Use If: 
- Need quick loading.
- If content is not updated frequently and there is not too much interactivity (Ex: blog, landing page)
#### Cons
- If the data is changed, users will be viewing data that is stale until the website is redeployed and the files are regenerated.

## Incremental Static Regeneration (ISR)
- Same as SSG but rebuilds the website on a specified interval. Hence, we can be more or less sure that the contents are updated.
- For example, if the interval is set to 5 seconds, the website will be rebuilt every 5 seconds.

## Reference
[Progressive web app structure | MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/App_structure)  
[서버사이드 렌더링](https://www.youtube.com/watch?v=iZ9csAfU5Os&ab_channel=%EB%93%9C%EB%A6%BC%EC%BD%94%EB%94%A9by%EC%97%98%EB%A6%AC)  
[What is a static site generator?](https://www.cloudflare.com/learning/performance/static-site-generator/)  
[Why I don't use Create React App - YouTube](https://www.youtube.com/watch?v=ToEp--KcXcA&ab_channel=DevEd)  
[Next.js SSR vs. SSG vs. ISR vs. CSR | Next.js Data Fetching - YouTube](https://www.youtube.com/watch?v=mWytwmxLKmo&ab_channel=DevWorld)  
