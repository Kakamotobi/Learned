# How to Apply CSS Effects on an Element while Hovering on another Element

## Restriction
The target element (element A) has to be a descendant of the hovered element (element B) in the HTML.

## Example
```
<div class="A">
  <div class="B">
    <div class="C"></div>
  </div>
  <div class="D"></div>
</div>
<div class="E">
  <div class="F"></div>
</div>
```
What works:

`.A:hover .B{CSS styles}`  
`.A:hover .D{CSS styles}`  
`.A:hover .C{CSS styles}`  
`.E:hover .F{CSS styles}`

What doesn't work:

`.A:hover .E{CSS styles}`  
`.A:hover .F{CSS styles}`  
`.E:hover .A{CSS styles}`  
`.E:hover .B{CSS styles}`  
`.E:hover .C{CSS styles}`  
`.E:hover .D{CSS styles}`
