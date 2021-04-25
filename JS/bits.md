# Random Bits

## Shorthand for Accessing Objects from API Objects
```
// pull out temp, temp_min, temp_max from data.main object and create a dom object out of them.
const {temp, temp_min, temp_max} = data.main;
```

## `innerText` vs. `textContent`
- `innerText` returns the visible text contained in a node, while `textContent` returns the full text.
- `innerText` is more performance-heavy.
### Example
```
<span>Hello <span>World</span></span>

// innerText returns "Hello".
// textContent returns "Hello World".
```
### Reference
[innerText vs. textContent](https://kellegous.com/j/2013/02/27/innertext-vs-textcontent/)
