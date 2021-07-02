# Useful DOM Stuff

## `document.body`
- **`.children`**
  - Use to select all the elements.
- **`.clientWidth`**
  - Width of the client's screen.
- **`.clientHeight`**
  - Height of the client's screen.

## `.getBoundingClientRect()`
- Returns a "DOMRect" object that provides information about the size of an element and its position relative to the viewport.
- Example
  ```js
  btn.getBoundingClientRect().right; //
  ```

## `window`
- **`.scrollY`**
  - The number of pixels that the document is currently scrolled vertically.
  - Returns the Y coordinate of the top edge of the current viewport.
  - Example
    ```
    if (window.scrollY) {
      window.scroll(0, 0) // reset the scroll position to the top left of the document.
    }
    
    window.scrollByPages(1);
    ```
  - Use this property to check that the document hasn't already been scrolled when using relative scroll functions such as `scrollBy()`, `scrollByLines()`, or `scrollByPages()`.
  - Same as `window.pageYOffset`
- **`.scrollX`**
  - The number of pixels that the document is currently scrolled horizontally.
- **`.scrollBy()`**
  - Scrolls the document in the window by the given amount. 

## Reference
[Document - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document)  
[Window - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window)
