# Script Tag Location
- When (where) in the HTML, is best to put a `<script>` tag?

## Table of Contents
- [The Different Options](#the-different-options)
- [Script Tag in `<head>` vs Script Tag in `<body>`](#script-tag-in-head-vs-script-tag-in-body)
- [`async` vs `defer`](#async-vs-defer)

## The Different Options
<p align="center">
  <img src="https://html.spec.whatwg.org/images/asyncdefer.svg" alt="Script Execution Timings" width="80%"/>
</p>

## Script Tag in `<head>` vs Script Tag in `<body>`
### Script Tag in `<head>`
- When parsing the HTML, upon encountering a `<script>` tag, the browser pauses on parsing the HTML, and proceeds to synchronously download and execute the script before resuming parsing and constructing the DOM.
- Scripts in `<head>` may lead to errors as the DOM may not yet be ready while the script is already downloaded and executing.
- It also means that if the script file is large, the page contents will require more time to be loaded onto the screen.
- Therefore, make sure to use the `defer` or `async` attributes according to your needs.
### Script Tag in `<body>`
- Ensures that the DOM is ready to be manipulated by the time a script starts to execute.
  - Ex: prevents JS from manipulating a target element that has not yet been parsed and inserted into the DOM.

## `async` vs `defer`
- Both `async` and `defer` indicates the browser to continue parsing the HTML while downloading the script.
- `async` immediately executes the script that has been downloaded even if the HTML is still being parsed.
  - _Could lead to errors since the DOM may not be fully constructed before the script executes._
  - The AMD module system is based on `async` scripts.
- `defer` executes the script only after the HTML is completely parsed and the DOM is fully constructed, but before the `DOMContentLoaded` is fired.
  - Native JavaScript modules (ESModules) are deferred by default. Therefore, the browser downloads the dependencies while parsing the HTML, and executes them once the DOM is constructed.
    - i.e. `type = "module"` attribute guarantees that the script executes after the DOM is ready.

## References
[HTML Standard](https://html.spec.whatwg.org/multipage/scripting.html#:~:text=4.12.1-,The%20script%20element,-%E2%9C%94MDN)  
[Where to put a script tag — into head or body end? | by Marian Čaikovski | Geek Culture | Medium](https://medium.com/geekculture/where-to-put-a-script-tag-into-head-or-body-end-b5b063058e0b)  
[<script>: The Script element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)  
