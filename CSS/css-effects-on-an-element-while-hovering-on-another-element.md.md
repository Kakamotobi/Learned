# How to Apply CSS Effects on an Element while Hovering on another Element

## Restriction
The target element (element A) has to be a descendant of the hovered element (element B) in the HTML.

## Example
```
<div class="A">
  <div class="B"></div>
  <div class="C"></div>
</div>
<div class="D">
  <div class="E"></div>
</div>
```
What works:

`.A:hover .B{CSS styles}`  
`.A:hover .C{CSS styles}`  
`.D:hover .E{CSS styles}`

What doesn't work:

`.A:hover .D{CSS styles}`  
`.A:hover .E{CSS styles}`  
`.D:hover .A{CSS styles}`  
`.D:hover .B{CSS styles}`  
`.D:hover .C{CSS styles}`  
