# Document Object Model (DOM)
> Connects web pages to scripts or programming languages by representing the structure of a document in memory.  
> **Represents a document with a logical tree. Each branch of the tree ends in a node, and each node contains objects.**  
> DOM methods allow programmatic access to the tree. With them, you can change the document's structure, style, or content.  
> Nodes can also have event handlers attached to them. Once an event is triggered, the event handlers get executed.
- Basically, JS "window"/access portal into the contents of the page.

## Terminologies
### Document Object
- The `document` object is the very top object (root) of the tree structure.
- It contains representations of all the contents on the page, and many methods and properties.
- `console.dir(document)`
### Model
- The tree structure in which objects have relationships according to the markup.
### Node
- Everything that can be changed in the `document` is a node.
  - Ex: elements, text within elements, HTML attributes.

## Methods
### Selecting Elements
- `document.getElementById("#")`
- `document.getElementsByTagName("")`
  - Returns HTMLCollection, which is *array-like*.
  - Example
    ```
    const allImages = document.getElementsByTagName("img");
    for (let img of allImages) {
      img.src = ""
    }
    ```
- `document.getElementsByClassName(".")`
- `document.querySelector("")`
  - Select a single element using id, name, class, attribute, and any CSS style selector.
  - *Selects the first match.*
- `document.querySelectorAll("")`
  - Selects all matches.
### Frequently Used Methods and Properties
- `.innerText`
  - Text/content between the opening and closing tag.
- `.textContent`
  - Same as `.innerText` but includes spaces according to the markup.
  - Returns every element/piece of content inside (both displayed and hidden).
- `.innerHTML`
  - Returns all the HTML markup of an element including tag names.
- `.getAttribute()`
  - Gets the value of the attribute directly from the HTML.
  ```
  const link = document.querySelector("a");
  
  // Option 1 - directly from the object.
  link.href;
  
  // Option 2 - directly from the HTML.
  link.getAttribute("href");
  ```
- `.setAttribute()`
  - Syntax: `.setAttribute(attributeName, newValue);`
-`.style`
  - Object containing all the corresponding CSS styles for the selected element.
  - *Empty at first (Doesn't contain styles from stylesheets).*
  - Using `.style` will and inline styles **(not ideal)**.
  - Camel cased.
  - Example
    - `link.style.textDecorationColor = "blue";`
  - `window.getComputedStyle(varName);`
    - Used in JS to access the CSS styles.
    - Example
      ```
      const h1 = document.querySelector("h1");
      window.getComputedStyle(h1).color;
      window.getComputedStyle(h1).fontSize;
      ```
- `.classList`
  - Returns the class(es) of the element.
  - A better way to apply styles to elements.
    - Create a CSS class with styles and add that class to the elements.
  - `.classList.toggle()`
    - Turn class on/off.
- `.parentElement`
  - Returns the parent element.
  - Ex: when user clicks on an element, a change occurs in the parent element.
- `.children`
  - Returns the children element.
- `.nextSibling`
  - Returns the corresponding next node.
  - *Some browsers automatically make white space / new lines into a text node.*
    - Ex: `#text` is a DOM node (not an HTML element).
- `.previousSibling`
  - Returns the corresponding previous node.
- `.nextElementSibling`
  - Returns the next element.
- `.previousElementSibling`
  - Returns the previous element.
- `.document.createElement("elementName")
  - Create a new empty DOM element.
- `.appendChild()`
  - Append the element in `()` as the last child of the location.
- `.append()`
  - Append multiple nodes/objects/elements/strings/etc. at the end of the element.
- `.prepend()`
  - Prepend multiple nodes/objects/elements/strings/etc. as the first child of the element.
- `.insertAdjacentElement()`
  - Insert an element next to another element.
  - Syntax: `targetElement.insertAdjacentElement(position, element);`
    - Position Values:
      - "beforebegin"
      - "afterbegin"
      - "beforeend"
      - "afterend"
- `.after()`
  - Insert an element after some other element.
- `.before()`
  - Insert an element before some other element.
- `.removeChild()`
  - Select the parent and remove a child from it.
  - Using `.removeChild()` to remove an element.
    - `element.parentElement.removeChild(element)`
- `.remove()`
  - Removes the selected node from the tree.
  - Example
    ```
    const firstLi = document.querySelector("li");
    const ul = document.querySelector("ul");
    
    // Option 1 - .removeChild()
    ul.removeChild(firstLi);
    // OR
    firstLi.parentElement.removeChild(firstLi);
    
    // Option 2 - .remove()
    firstLi.remove();
    ```
## DOM Events
### Some Events
- Click, drag, drop, hover, scroll, form submission, key presses, focus/blur, mouse wheel, double click, copying, pasting, audio start, screen size, printing.
### 3 Ways to respond to user inputs and actions.
#### Inline Events
- Directly in the HTML markup.
##### Example
```
<button onclick="alert('clicked!'); alert('clicked again!')">Click Me</button>
```
#### Property Approach
- Create selector/variable with method properties (ex: .onclick) and store function in it.
##### Example
```
const btn = document.querySelector("button");

// Option 1
btn.onclick = function () {
  console.log("clicked");
}

// OR predefine function to run on this event
function clicked() {
  console.log("clicked");
}
btn.onclick = clicked;
```
#### `.addEventListener()`
- Specify the event type and a callback function to run upon that event.
- *Can have multiple callback functions for the same event.*
- `.removeEventListener()`
  - Remove event listener.
##### Example 1
```
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  console.log("clicked!");
});
```
##### Example 2
```
const btn = document.querySelector("button");

btn.addEventListener("click", clicked);

function clicked() {
  console.log("clicked!");
}
```
##### Example 3
```
// Listening for events on the window as a whole.
window.addEventListener("", function(){});`
```

### Event Object and Keyword Events
#### Event Object
- The event object is always automatically passed in to the callback.
- Contains information about the interaction with the event object.
  - Ex: clientX, clientY indicates where the click occured relative to the window.
    ```
    .addEventListener("click", function(evt) {
      console.log(evt);
    });
    ```
- The event object can be used to have something appear on the page location where the user clicked.
#### Keyboard Events
- Often need to rely on the event object when working with keyboard events.
##### Example
```
const input = document.querySelector("input");

input.addEventListener("keydown", function() {
  console.log("keydown!");
});
input.addEventListener("keyup", function() {
  console.log("keyup!");
});
```
##### `evt.key` vs `evt.code`
###### `evt.key`
- Use if the character that was generated is important.
- Ex: a.
###### `evt.code`
- Use if the location/position of the key that was pressed on the keyboard is needed.
- Ex: KeyA.
- Ex: `wasd` for moving.
##### `switch` statement, `case`, `break`
- Listening for specific keyboard events.
##### Example
```
window.addEventListener("keydown", function(evt) {
  switch (evt.code) {
    case "ArrowUp":
      console.log("UP!");
      break;
    case "ArrowDown":
      console.log("DOWN!");
      break;
    case "ArrowLeft":
      console.log("LEFT!");
      break;
    case "ArrowRight":
      console.log("RIGHT!");
      break;
    default:
      console.log("IGNORED!");
  }
});
```

## Reference
[Introduction to the DOM | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
