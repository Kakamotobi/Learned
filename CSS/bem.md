# Block Element Modifier (BEM)
- Allows other developers to easily identify which classes are responsible for what and how they depend on one another.

## Taking Apart BEM

### Block
- A stand-alone entity.
- Thought of as a parent.
- Ex: `.btn {}`

### Element
- A list item or a title that is tied to the block that it is in.
- Thought of as child elements.
- Ex: `.btn__price {}`

### Modifier
- A flag on a block or element that changes the styling or behavior.
- Used to manipulate the block so that we can theme or style that particular component without inflicting changes on a completely unrelated module.
- Ex: `.btn--orange {}`
- Ex: `.btn--big {}`

## Options to Consider for JS/DOM Manipulation
1. Set a class (`js-`) dedicated to JS if also used in DOM.
    - Ex: `<div class="site-navigation js-site-navigation"></div>`
2. Make use of the `rel` attribute.
    - Ex: `<div class="site-navigation" rel="js-site-navigation"></div>`

### Markup Example
```
<div class="card card--light">
  <img class="card__image" src="" alt="">
  <h2 class="card__title">Light Card</h2>
  <p class="card__body">Lorem ipsum blah</p>
</div>

<div class="card card--dark">
  <img class="card__image" src="" alt="">
  <h2 class="card__title">Dark Card</h2>
  <p class="card__body">Lorem ipsum blah</p>
</div>
```

## Reference
[BEM 101 | CSS-Tricks](https://css-tricks.com/bem-101/)
