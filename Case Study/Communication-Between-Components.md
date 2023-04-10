# Communication Between Components

## Table of Contents
- [Custom Events](#custom-events)

## Custom Events
- Custom events can be used to communicate specific information between components.
### Example - ComponentA &rarr; ComponentB
```js
// ComponentA

const woofEvt = new CustomEvent("woof", {
  detail: {
    name: "Spiky",
  },
});

componentB.dispatchEvent(woofEvt);
```
```js
// ComponentB

this.addEventListener("woof", (evt) => {
  console.log(evt.detail); // { name: "Spiky" }
});
```

## Reference
[CustomEvent: CustomEvent() constructor - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent)  
