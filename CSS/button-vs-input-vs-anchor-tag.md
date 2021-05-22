# Button vs. Input vs. Anchor Tag
- All three elements can be made to look and behave the same. Choosing either one boils down to semantics.

## Button
- Released as an alternative to inputs that are much easier to style.
  - Unlike inputs, a button’s label is determined by its content (can nest images, paragraphs, headers, etc.).
  - Can also have `::before` and `::after` pseudo elements.
- `type` can be set to `submit` (default), `reset`, or `button`.
## Input
- Represents a data field in which `type` can be set to different types of data the input controls.
- Button-related `input` types:
  - **`<input type=“submit”>`**
    - Button that submits a form when clicked on.
  - **`<input type=“image”>`**
    - Button that submits a form when clicked on. But also has a `src` attribute to display an image.
  - **`<input type=“file”>`**
    - Used to upload files and is shown as a label and a button.
    - There’s no good cross-browser way to style file inputs, so usually, set its `display` to `hidden` and use a second button to trigger it.
  - **`<input type=“reset”>`**
    - Button that resets a form.
  - **`<input type=“button”>`**
    - Button with no default behavior.
    - Can be used to add non-standard behavior to a form using JS.

## Anchor Tag
- Represents a hyperlink, which is a resource that a user can navigate to (a new page) or download in a browser.

## Reference
[The Difference Between Anchors, Inputs and Buttons](https://davidwalsh.name/html5-buttons)  
[The Input (Form Input) Element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)
