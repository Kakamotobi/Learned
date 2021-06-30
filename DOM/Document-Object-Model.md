# Document Object Model (DOM)
> Connects web pages to scripts or programming languages by representing the structure of a document in memory.  
> **Represents a document with a logical tree. Each branch of the tree ends in a node, and each node contains objects.**  
> DOM methods allow programmatic access to the tree. With them, you can change the document's structure, style, or content.  
> Nodes can also have event handlers attached to them. Once an event is triggered, the event handlers get executed.  

> *The web browser provides an API for accessing the HTML document in a tree structure known as the DOM.*  
> *The combination of JS and the DOM is what allows us to create interactive, dynamic websites.*
> *JS is the client-side scripting language taht connects to the DOM in an internet browser.*

## Table of Contents
- [Terminologies](#terminologies)
- [Methods](#methods)
  - [Selecting Elements](#selecting-elements)
  - [Frequently Used Methods and Properties](#frequently-used-methods-and-properties)
    - [Traversing the DOM](#traversing-the-dom)
    - [Making Changes to the DOM](#making-changes-to-the-dom)
    - [Modify Attributes, Classes, and Styles in the DOM](#modify-attributes-classes-and-styles-in-the-dom)
- [DOM Events](#dom-events)
  - [3 Ways to Add Events](#3-ways-to-add-events)
    - [Inline Events](#inline-events)
    - [Property Approach](#property-approach)
    - [.addEventListener()](#addeventlistener)
  - [Event Object and Keyboard Events](#event-object-and-keyboard-events)
  - [Form Events and .preventDefault()](#form-events-and-preventdefault)
  - [Change Event and Input Event](#change-event-and-input-event)
  - [Event Bubbling and .stopPropagation()](#event-bubbling-and-stoppropagation)
  - [Event Delegation and evt.target](#event-delegation-and-evttarget)

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
#### Traversing the DOM
- `.parentElement`
  - Returns the parent element.
  - Ex: when user clicks on an element, a change occurs in the parent element.
- `.children`
  - Returns the children element in an array-like object.
- `.previousSibling`
  - Returns the corresponding previous node.
- `.nextSibling`
  - Returns the corresponding next node.
  - *Some browsers automatically make white space / new lines into a text node.*
    - Ex: `#text` is a DOM node (not an HTML element).
- `.previousElementSibling`
  - Returns the previous element.
- `.nextElementSibling`
  - Returns the next element.
#### Making Changes to the DOM
- `.createElement("elementName")`
  - Create a new empty DOM element.
- `.innerText`
  - Text/content between the opening and closing tag.
- `.textContent`
  - Same as `.innerText` but includes spaces according to the markup.
  - Returns every element/piece of content inside (both displayed and hidden).
- `.innerHTML`
  - Returns all the HTML markup of an element including tag names.
- `.append()`
  - Append multiple nodes/objects/elements/strings/etc. at the end of the element.
- `.prepend()`
  - Prepend multiple nodes/objects/elements/strings/etc. as the first child of the element.
- `.appendChild()`
  - Append the element in `()` as the last child of the location.
- `.replaceChild()`
  - Replace an existing node with a new node.
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
- `.insertBefore()`
  - Insert a node into the parent element before a specified sibling node.
- `.before()`
  - Insert an element before some other element.
- `.after()`
  - Insert an element after some other element.
- `.insertAdjacentElement()`
  - Insert an element next to another element.
  - Syntax: `targetElement.insertAdjacentElement(position, element);`
    - Position Values:
      - "beforebegin"
      - "afterbegin"
      - "beforeend"
      - "afterend"
#### Modify Attributes, Classes, and Styles in the DOM
##### Attributes
- `.hasAttribute()`
  - Returns a true or false.
- `.getAttribute()`
  - Gets the value of the attribute directly from the HTML.
  ```
  const link = document.querySelector("a");
  
  // Option 1 - directly from the object.
  link.href;
  
  // Option 2 - directly from the HTML.
  link.getAttribute("href");
  ```
- `.removeAttribute()`
  - Remove the specified attribute.
- `.setAttribute(attributeName, newValue)`
  - Set a new value for the specified attribute.
##### CSS Classes
- `.className`
  - Gets or sets class value. 
- `.classList`
  - Returns the class(es) of the element.
- `.classList.contains()`
  - Checks if class value exists.
- `.classList.add()`
  - Adds one or more class values.
- `.classList.remove()`
  - Removes a class value.
- `.classList.toggle()`
  - Toggles a class on or off.
##### Styles
- `.style`
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
- `getComputedStyle(element)`
  - Returns all the styles applied from CSS on that element.

## DOM Events
### Some Events
- Click, drag, drop, hover, scroll, form submission, key presses, focus/blur, mouse wheel, double click, copying, pasting, audio start, screen size, printing.
### 3 Ways to Add Events
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
##### Events
- "click"
- "dblclick"
- "mouseenter"
- "mouseleave"
- "mousemove"

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

### Event Object and Keyboard Events
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

### Form Events and `.preventDefault()`
#### `.preventDefault()`
- Prevents the default behavior that will happen as a result of the event.
- *After forms are submitted, the web page continues to the next page (action) and doesn't stay in the original page.*
##### Example
```
const form = document.querySelector("form");

form.addEventListener("submit", function(evt) {
  evt.preventDefault();
});
```
#### `.value` attribute
- Direct property to access the content/value of something.
##### Example
```
const tweetForm = document.querySelector("#tweetForm");

tweetForm.addEventListener("submit", function(evt) {
  evt.preventDefault();
  
  const usernameInput = document.querySelector(".username");
  const tweetInput = document.querySelector(".tweet");
  
  console.log(usernameInput.value, tweetInput.value);
```
#### Selecting Different Elements in the Form Element
- `.elements`
  - Form property containing HTML name attributes.
  - *Can also iterate over the elements.*
##### Example
```
const tweetForm = document.querySelector("#tweetForm");
const tweetsContainer = document.querySelector("#tweets");

// Upon form submission
tweetForm.addEventListener("submit", function(evt) {
  evt.preventDefault();
  const usernameInput = form.elements.username.value;
  const tweetInput = form.elements.tweet.value;
  addTweet(usernameInput.value, tweetInput.value);
  usernameInput.value = "";
  tweetInput.value = "";
});

// Function for posting a new tweet
const addTweet = (usernameInput, tweetInput) => {
  const newTweet = document.createElement("li");
  const bTag = document.createElement("b");
  bTag.append(usernameInput);
  newTweet.append(bTag);
  newTweet.append(` - ${tweetInput}`);
  tweetsContainer.append(newTweet);
}
```

### Change Event and Input Event
#### Change Event
- Only fires when the input is blurred (i.e., leaving the input field).
  - When the user leaves the input and the input is different from before.
- Keys that don't impact the value of the input (ex: shift, tab, arrow keys, etc.) do not fire the change event.
#### Input Event
- Fires whenever what's in the input changes (not on blur).
- Keys that don't impact the value of the input (ex: shift, tab, arrow keys, etc.) do not fire the change event.
##### Example
```
input.addEventListener("input", function () {
  h2.innerText = input.value;
});
```

### Event Bubbling and `.stopPropagation()`
#### Event Bubbling
- When an event occurs on an element, it first runs the event handlers on it, then on its parent, then all the way up on other ancestors.
- i.e., parent/ancestor's event handlers also trigger on an element.
#### `.stopPropagation()`
- After running the current element's event handlers, stop and don't run parent/ancestors' handlers.
#### Example
```
const container = document.querySelector("#container"); // Parent
const btn = document.querySelector("button); // Child

// When clicking on either the container or btn, the background color changes.
container.addEventListener("click", function() {
  this.style.backgroundColor = "blue";

// Now, when clicking the btn, the background color doesn't change.
btn.addEventListener("click", function(evt) {
  evt.stopPropagation();
});
```

### Event Delegation and `evt.target`
- Add event listener to the parent of the target element in order to apply the event listener to the target elements created in the future.
- Ex: for elements that may not be on the page at the time the event listeners were added.
#### `evt.target`
- Reference to the object onto which the event was dispatched.
#### Example
```
const tweetForm = document.querySelector("#tweetForm");
const tweetsContainer = document.querySelector("#tweets");

tweetForm.addEventListener("submit", function (evt) {
  evt.preventDefault();
  const username = tweetForm.elements.username;
  const tweet = tweetForm.elements.tweet;
  addTweet(username, tweet);
  username.value = "";
  tweet.value = "";
});

// Function for adding new tweet
const addTweet = (username, tweet) => {
  const newTweet = document.createElement("li");
  const bTag = document.createElement("b");
  const dltButton = document.createElement.("button");
  dltButton.innerText = "Delete";
  dltButton.classList.add("dltButton");
  bTag.innerText = username.value;
  newTweet.append(bTag);
  newTweet.append(` - ${tweet.value}`);
  newTweet.append(dltButton);
  tweetsContainer.append(newTweet);
};

// Event Delegation
tweetsContainer.addEventListener("click", function (evt) {
  // In the tweetsContainer <ul> , when a button is clicked, remove its parentNode <li>.
  evt.target.nodeName === "BUTTON" && evt.target.parentNode.remove();
});
```

## Reference
[Introduction to the DOM | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)  
[Understanding the DOM - Document Object model](https://assets.digitalocean.com/books/understanding-the-dom.pdf)
