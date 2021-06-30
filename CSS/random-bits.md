# Random Bits

## `innerText` vs. `textContent`
- `innerText` returns the visible text contained in a node, while `textContent` returns the full text as it is.
- `innerText` is more performance-heavy.
### Example
```
<span>Hello <span>World</span></span>

// innerText returns "Hello".
// textContent returns "Hello World".
```
### Reference
[innerText vs. textContent](https://kellegous.com/j/2013/02/27/innertext-vs-textcontent/)

## [OPINION] "Wrapper" vs. "Container"
### Wrapper
- Something that wraps around a single object to provide more functionality and interface to it.
- Ex: general page wrapper, applying a sticky footer.
### Container
- Generally used for structures that can contain more than one element.
  - Sometimes necessary to implement a behavior or styling of multiple components.
- It serves the purpose of grouping elements both semantically and visually.
### Reference
[The Best Way to Implement a "Wrapper" in CSS | CSS-Tricks](https://css-tricks.com/best-way-implement-wrapper-css/)
