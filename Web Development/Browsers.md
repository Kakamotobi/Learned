# Browsers
> A Web browser or browser is a program that retrieves and displays pages from the Web, and lets users access further pages through hyperlinks. | MDN
- Takes code and displays the web page.

## Table of Contents
- [Web Browser High-Level Structure](#web-browser-high-level-structure)
- [How Web Browsers Display Web Pages](#how-web-browsers-display-web-pages)
    - [Critical Rendering Path](#critical-rendering-path)
    - [The Pixel Pipeline](#the-pixel-pipeline)
- [Popular Web Browsers](#popular-web-browsers)

## Web Browser High-Level Structure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/web-browser-structure.png" alt="Web Brower Structure" width="80%" /><br>
  <em>Things are actually more complex than this.</em>
</p>

### User Interface
- Every aspect of the browser such as the address bar, back/forward button, refresh button, bookmarks, and everything else excluding the window for viewing the requested page.
### Browser Engine
- Marshals the actions between the browser UI and the rendering engine.
- When the user enters something in the address bar, the browser queries this and controls the rendering engine to bring the response.
### Rendering Engine
- Two Main Objectives
  - Display the requested content by parsing HTML and CSS.
  - Efficiently render updates whenever needed (Ex: user inputs, animation, load data).
- Not all browsers use the same rendering engine, which is why we sometimes see inconsistencies on how a web page looks between browsers.
  - These engines have their own implementation of how to render the page.
- [**Critical Rendering Path**](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Browsers.md#critical-rendering-path)
### Networking
- Handling network calls such as HTTP requests for fetching resources, etc.
#### Browser Fetching Actions
- Search bookmarks and history to make meaningful suggestions as user types in the address bar.
- Parse the URL and retrieve protocol.
- If NON-ASCII exists, use encoding to convert the URL.
- Check HTTP Strict Transport Security (HSTS) list. If URL exists in that list, append HTTPS. Otherwise, append HTTP.
- DNS lookup order.
  - Browser DNS cache → OS cache → DNS server in local router or ISP router.
- Get the IP address from either the DNS server or default getaway using Address Resolution Protocol (ARP).
- Open port 53 and through User Datagram Protocol (UDP), communicate with the DNS server.
  - Either TCP or UDP is used based on the size of the response.
- If the DNS server is unaware of the entered address, the browser searches and returns the top 10 results using the default search engine.
- Else if the DNS server is aware of the entered address, the http(port 80)/https(port 443) request starts and TCP socket connection is requested.
- The request reaches: 
  - the Network Layer to fill TCP header.
  - Then, the Transport Layer to fill IP header.
  - Then, the Data Link Layer to fill Ethernet Frame header.
- The data packet flows in the network in digital or cellular.
- 3-way handshake occurs between the server and client. Data is sent to the client.
- The Transport Layer handshake occurs. The browser receives the data in chunks.
### JavaScript Interpreter
- The JavaScript engine/interpreter that reads and executes JavaScript scripts.
- Different browsers use different JavaScript interpreters. Therefore, support for JavaScript features differ by browsers.
- Internal JavaScript implementations may also differ.
  - Ex: Mozilla uses merge sort for `Array.sort()` while Chrome uses a combination of quick sort and insertion sort for smaller arrays.
### UI Backend
- Draws basic widgets such as combo boxes and windows.
- Generic interface that is not platform specific.
### Data Persistence
- Browsers provide various storage mechanisms such as local storage, session storage, indexedDB, web SQL, FileSystem.
- It is used to manage data such as cache, cookies, bookmarks, preferences, etc.
- It is a small database created on the local drive of the computer where the browser is installed.

## How Web Browsers Display Web Pages
### 1) User makes an HTTP request.
### 2) The browser recieves an HTTP response from the server.
### 3) Browser Engine
- Act as a marshall to direct actions between the UI or servers and the rendering engine.
### 4) Rendering/Layout Engine
#### Critical Rendering Path
- The Rendering Engine parses the HTML document and constructs the **DOM Tree**.
  - The browser requests stylesheets and scripts that it encounters.
- Then, it parses CSS and constructs the **CSSOM Tree**. 
- Then, DOM nodes are associated with their corresponding styles from the CSSOM to construct the **Render Tree**.
  - The content and styles of all visible content has been collected.
- Then, execute a **layout process** where each node is positioned on the screen with coordinates.
  - A.k.a. **reflow**.
  - Calculate the exact position and size of each node within the viewport according to the box model.
- Then, the Render Tree is traversed to paint the individual nodes to the screen using the UI Backend Layer.
  - A.k.a. **painting/rasterizing process**.
  - The browser typically makes different layers where painting is done. The browser combines all the layers in the correct order into one layer to display on the screen.
    - This is known as **Composite Layers**.
#### The Pixel Pipeline
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/pixel-pipeline.png" alt="Pixel Pipeline" width="80%" />
</p>

- **JavaScript**
  - Trigger style changes (Ex: animation, adding DOM elements, etc.).
  - Style triggers are not only made by JavaScript.
    - CSS animations, transitions and web animation APIs are also used.
- **Style**
  - Calculate styles by finding CSS rules that apply to the elements.
- **Layout**
  - Calculate position and space of the elements.
  - This process can be demanding for the browser (Ex: the parent element's styles affect children elements' styles).
- **Paint**
  - Draw out the visuals of the elements, which is done on multiple layers.
- **Composite**
  - Combine all the layers into one to finally display onto the screen.
- *There are three ways that the pixel pipeline plays out.*
##### Layout Property
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/pixel-pipeline.png" alt="Layout property pixel pipeline" width="80%" />
</p>

- Occurs when the size or position of an element changes or the viewport size changes.
- Since the layout needs to be recalculated, elements have to be repainted, and go through the last process of compositing.
##### Paint Property
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/paint-only.png" alt="Paint property pixel pipeline" width="80%" />
</p>

- Occurs when changes are made to styles such as background, color, shadow.
- i.e., changes that do not lead to a layout change.
- Since elements have to be repainted, the compositing process is also required.
##### Composite Change
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/composite-only.png" alt="Composite pixel pipeline" width="80%" />
</p>

- Occurs when the change does not require neither layout nor paint.
- Render moves straight to compositing layers.
##### Useful Website
- Shows how each rendering engine updates UI for each CSS property.
- [CSS Triggers](https://csstriggers.com/)
##### Notes
- Reducing the time of the critical rendering path can help reduce the overall time it takes for the browser to display a web page.
- Use CSS properties that are more easy on the rendering engine.
  - Ex: use `transform` instead of `right`, etc.
### 5) [JavaScript Engine/Interpreter](https://github.com/Kakamotobi/Learned/blob/main/JS/JavaScript.md#javascript-engine)
- Scripts are read and executed.

## Popular Web Browsers
| Browser                                         | Rendering Engine         | JS Engine/Interpreter |
| ----------------------------------------------- | ------------------------ | --------------------- |
| **Chrome/Chromium** (Edge, Brave, Whale, Opera) | Blink                    | V8                    |
| **Firefox/Mozilla**                             | Gecko, Servo             | SpiderMonkey          |
| **Safari/Webkit**                               | WebKit                   | JavaScriptCore        |
| **IE/Old Edge**                                 | Trident(~IE11), EdgeHTML | Chakra                |

## Reference
[Browser - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Browser)  
[Rendering engine - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Rendering_engine)  
[How Web Browsers Work — Behind the scene Architecture, Technologies, and Internal Working | by Upender Bhardwaj | Web God Mode | Medium](https://medium.com/web-god-mode/how-web-browsers-work-behind-the-scene-architecture-technologies-and-internal-working-fec601488bfa)  
[How Browsers Work: Behind the scenes of modern web browsers - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)  
[Rendering Performance](https://web.dev/rendering-performance/)  
[Browser Engines... Chromium, V8, Blink? Gecko? WebKit? - Medium](https://medium.com/@jonbiro/browser-engines-chromium-v8-blink-gecko-webkit-98d6b0490968)  
[Browsers: Rendering Engines & JS Engines - Medium](https://medium.com/@acparas/browsers-rendering-engines-js-engines-bea42b77a182)  
[How Web Browsers Funciton - YouTube](https://www.youtube.com/watch?v=z0HN-fG6oT4&ab_channel=OpenCanvas)  

