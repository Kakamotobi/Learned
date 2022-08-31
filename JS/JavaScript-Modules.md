# JavaScript Modules

## Table of Contents
- [A Bit of History (JS Before Modules)](#a-bit-of-history-js-before-modules)
- [Introduction of Modularity in JS](#introduction-of-modularity-in-js)
- [Making JS Modules Work in the Browser](#making-js-modules-work-in-the-browser)
- [Module Scripts vs Standard Scripts](#module-scripts-vs-standard-scripts)
- [Isomorphic Modules](#isomorphic-modules)
- [Some JS Module Systems/Standards](#some-js-module-systemsstandards)
  - [CommonJS (CJS)](#commonjs-cjs)
- [Bundling Modules](#bundling-modules)

## A Bit of History (JS Before Modules)
- JavaScript did not used to have modularity (a way to import/export modules).
  - i.e. code had to be written in a single JS file.
- As JavaScript began to be used more extensively in web applications and websites, there became a need to split JS programs into separate modules to be used when needed.
  - Node.js (first released in 2009) was capable of doing so for a long time.

## Introduction of Modularity in JS
- Many JS libraries and frameworks began to be rise in an attempt to enable module usage in JS.
### `.mjs` Extension
- Extension that can be used for module files.
- It is good for clarity, i.e. it makes it clear which files are modules, and which are regular JavaScript.
- It ensures that your module files are parsed as a module by runtimes such as Node.js, and build tools such as Babel.
- *Not all servers set the correct type for `.mjs` files as of yet.*
  - Therefore, you could choose to use `.mjs` during development and then convert them into `.js` when building for production.

## Making JS Modules Work in the Browser
- The server needs to serve the JS modules with a `Content-Type` header with a JS MIME type such as `text/javascript` or else it will result to an error and the browser will not run the files.
- For a module to work, we need to denote that the script is a module and import it in our HTML.
  - Ex: `<script type="module" src="index.js"></script>`.

## Module Scripts vs Standard Scripts
- Loading the HTML file locally (`file://`) will result in CORS errors for JS module scripts (security requirements). Therefore, module scripts need to be tested/developed through a server.
- Module scripts use strict mode by default.
- Module scripts are deferred by default (no need to use the `defer` attribute).
- Module scripts are executed once, despite being referenced in separate scripts.
- Functions and variables in modules are *module-scoped*.
  - Modules are imported into the scope of a single script, not the global scope. But globally defined variables can be accessed within modules.
- Modules can also import/export classes.
  - Example
    ```js
    class Character {
      constructor(name, species, age) {
        // ...
      }
      greet() {
        console.log(`Hello. I'm ${this.name}.`);
      }
    }
    
    export Character;
    ```
    ```js
    import { Character } from "./modules/Character.js";
    
    const tom = new Character("Tom", "cat", 3);
    tom.greet(); // "Hello. I'm Tom."
    ```
- Modules can use top level `await` keyword, hence the module acts like a big asynchonous function.
  - i.e. code can be evaluated before using in parent modules, but doesn't block sibling modules from loading.

## Isomorphic Modules
- Not all JS code can be run in every environment.
  - Ex: `window` is only available in browsers, `process` is only available in a Node.js environment.
- Isomorphic modules exhibit the same behavior in every runtime.
### 3 Ways to Achieve an Isomorphic Module
- Separate the modules into "core" and "binding".
  - "core" should focus on pure JS logic (platform-non-specific).
    - Ex: computing a hash. No DOM, network, etc., and expose utility functions.
  - "binding" should read from and write to the global context (platform-specific).
    - Ex: "browser binding" can read a value from an input box. "Node binding" can read a value from `process.env`. Either way, the value will be used in the same core function and handled in the same manner.
- Detect whether a particular global variable exists or not before using it.
- Use a polyfill to provide a fallback for missing features.

## Some JS Module Systems/Standards
### CommonJS (CJS)
- Introduced in 2009.
- Modules are imported synchronously.
- CJS imports return a *copy* of the imported object.
- CJS can import from a library in `node_modules` or in your local directory.
- CJS is not supported by browsers. Hence, it needs to be transpiled and bundled.
- Node.js supports the CJS module system.
- Example
  ```js
  // Import
  const iAmModule = require("./iAmModule.js");
  
  // Export
  module.exports = function iAmModule() {
    // ...
  }
  ```
### Asynchronous Module Definition (AMD)
- Modules are imported asynchronously.
- AMD is intended for the frontend (cf. CJS for backend).
- AMD has a less intuitive syntax.
### Universal Module Definition (UMD)
- UMD works for both frontend and backend.
- UMD is more of a pattern to configure several module systems, unlike CJS or AMD.
- Usually used as a fallback (if ESM doesn't work) when using bundlers like Rollup and Webpack.
### ESModules (ESM)
- Introduced with ES2015 (ES6).
- ESM is ECMAScript's official standardized module system.
- ESModules is a more modern approach, supported in both browsers and Node (since Node 13).
- Best of both worlds: simple syntax (CJS) and asynchronous import (AMD).
- Tree-shakable.
  - Import only what you need (shaking off the rest).
- Need to add `"type": "module"` to `package.json` for it to work on Node.js.

## Bundling Modules
- Modular code makes our codebase more organized easier to develop. However, modular code is only preferable in development. In production, modular code means the browser has to make requests for each JS script, which can hinder the site's performance.
- This is why module bundlers are important as they take all the modules and combine them into a single file, which is then used as the production code.
- Some module bundlers include Webpack, Rollup, Snowpack, Parcel.
- *Make sure to replace the script tag in our HTML to the bundled file (not the dev file).*

## Reference
[JavaScript modules - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)  
[ES modules: A cartoon deep-dive - Mozilla Hacks - the Web developer blog](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)  
[What the heck are CJS, AMD, UMD, and ESM in Javascript? - DEV Community](https://dev.to/iggredible/what-the-heck-are-cjs-amd-umd-and-esm-ikm)  
[Modules in JavaScript - CommonJS and ESmodules Explained](https://www.freecodecamp.org/news/modules-in-javascript/)  
