# Web APIs

## Definition
- Application Programming Interface (API)
- Constructs made available in programming languages to allow developers to create complex functionality more easily.

## How they work
- The data that the API uses are contained in JS object properties.
- The functionalities that the API makes available are contained in JS object methods.

## APIs in Client-side JS
*Not a part of the JS language itself.*
- Web APIs are able to handle certain tasks (API functions), given by JS, in the background.

### Browser APIs
- Browsers come with APIs, which are methods that we can call from JS.
- Ex:
  - `setTimeout()`
  - Web Audio API
    - Allows manipulation of audio (volume, etc.) in the browser.
- The JS call stack recognizes these web API functions and passes them off to the browser to take care of.
- Once the browser finishes those tasks, they return and are pushed onto the stack as a callback.
- [More browser APIs](https://developer.mozilla.org/en-US/docs/Web/API)

### Third-party APIs
- Not built-in to the browser by default.
- Constructs built into third-party platforms that allows us to use some of those platform's functionality in our own web pages.
- Code and information has to be retrieved from somewhere on the Web.
- Ex: Google Maps

## Reference
[Introduction to Web APIs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)
