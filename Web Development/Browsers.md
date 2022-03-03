# Browsers
> A Web browser or browser is a program that retrieves and displays pages from the Web, and lets users access further pages through hyperlinks. | MDN

## Popular Web Browers
| Browser               | Browser Engine | JS Engine      |
| --------------------- | -------------- | -------------- |
| **Chrome**            | Blink          | V8             |
| **Edge**              | EdgeHTML       | Chakra         |
| **Firefox**           | Gecko          | SpiderMonkey   |
| **Safari**            | WebKit         | JavaScriptCore |
| **Opera**             | Blink          | V8             |
| **Internet Explorer** | Trident        | Chakra         |

## How Web Browsers Function
### 1) User makes an HTTP request
### 2) The browser recieves an HTTP response from the server
### 3) Rendering/Layout Engine
- **Responsible for displaying the visual representation of a web page (interprets HTML, CSS).**
- Not all browsers use the same rendering engine, which is why we sometimes see inconsistencies on how a web page looks between browsers.
  - These engines have their own implementation of how to render the page.
#### Critical Rendering Path
- The Rendering Engine reads the HTML document and constructs the **DOM tree**.
- Then, the styles associated with each DOM node are passed by the engine to construct the **CSSOM tree**.
- Then, DOM and CSSOM are combined to create the **Render Tree**.
- Then, go through a layout process where each node is positioned on the screen with coordinates.
- Then, the Render Tree is traversed to paint the individual nodes to the screen using the UI Backend Layer.
### 4) Browser Engine
- **Responsible for managing all render processes and displaying UI.**
- Acts as a marshall to direct actions between the UI or servers and the rendering engine.
- Ex: Blink, WebKit, Gecko.
### 5) Network
- The browser has to communicate over the network to recieve and send information.
### 6) JavaScript Engine/Interpreter
- **Responsible for interpreting, compiling, and executing JavScript code.**
- JS Engine is not aware of the DOM.
- Each browser has its own JS engine that interprets interactive logic and functionalities.
- Ex: V8 (also used in Node.js), SpiderMonkey, JavaScriptCore.
### 7) Data Storage
- Browsers have a few ways to store data.
- Ex: Cookies, Local Storage.

## Reference
[Browser - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Browser)  
[Rendering engine - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Rendering_engine)  
[Browser Engines... Chromium, V8, Blink? Gecko? WebKit? - Medium](https://medium.com/@jonbiro/browser-engines-chromium-v8-blink-gecko-webkit-98d6b0490968)  
[Browsers: Rendering Engines & JS Engines - Medium](https://medium.com/@acparas/browsers-rendering-engines-js-engines-bea42b77a182)  
[How Web Browsers Funciton - YouTube](https://www.youtube.com/watch?v=z0HN-fG6oT4&ab_channel=OpenCanvas)  

