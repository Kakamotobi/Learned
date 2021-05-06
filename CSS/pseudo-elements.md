# Pseudo Elements
> Keyword added to a selector that lets you style a specific part of the selected element(s).

## Requirements
- `content: ""`
  - *Cannot have a pseudo element on empty tags (elements that don't have content. Ex: `input`).*
- Set `display:`

## `::before` and `::after`
- These are created inside of the element (not separate elements).
- Only allowed one of each per element.
### Example 1
```
<style>
// Any element with the .required class will have an *.
.required::after {
  content: "*";
}
</style>

<body>
  <label class="required">Name</label>
  <input type="text" required>
  <button>Submit</button>
</body>
```
### Example 2
- Add new things to an element (ex: tooltip).
```
<style>
// Set position to relative to absolutely position the pseudo element.
[data-tooltip] {
  position: relative;
}
// When hovering over button, display tooltip.
[data-tooltip]:hover::after {
  // Set content to be the attribute.
  content: attr(data-tooltip);
  // Position the tool tip.
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  margin-top: 5px;
  padding: 5px;
  background-color: lightgrey;
}
</style>

<body>
  <label class="required">Name</label>
  <input type="text" required>
  <button data-tooltip="I am tooltip">Submit</button>
</body>
```

## Some Other Pseudo Elements
- `::first-letter`
- `::first-line`

## Reference
[Pseudo-elements - CSS | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)
