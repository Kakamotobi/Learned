# Margin Collapse

## What is it?
When the margin of two blocks combine together to the largest margin from the two.  
*Only happens vertically (top and bottom margins).*

## When does it occur?
1. Adjacent Block Siblings
2. Empty Block
    - "Empty" means there is no property set for the height of the block (ex: `height`, `min-height`, `padding`, `border` or `inline` content).
    - Therefore, a block's `margin-top` and `margin-bottom` meet and causes its margin to collapse, which can lead to multiple margin collapses with adjacent blocks.
    - **The margin collapses to the largest of its margin-top or margin-bottom.**
3. No Content Separating Block Parent and its Block Descendants.
    - Margins, by definition, are space between contents and hence need some sort of boundary (ex: `border`, `padding`, `inline` content).
    - Parent `margin-top` & Descendant `margin-top`
      - In this case, whether any height property is set or not is disregarded.
      - Therefore, if there is no `border`, `padding` or `inline` content between the `margin-top` of the parent and descendant, the **descendant's margin always collapses to the outside of the parent**. 
    - Parent `margin-bottom` & Descendant `margin-bottom`
      - If there is no `border`, `padding`, `inline` content, `height`, `min-height`, or `max-height` between the `margin-bottom` of the parent and descendant, the **descendant's margin always collapses to the outside of the parent**. 

## Exceptions
- When the block is set to `position: absolute`.
- When the block is set to `float: left/right` and `clear` is not set.
- When parent block is set to `display: flex`, its children's (flexbox items) margin doesn't collapse.
- When parent block is set to `display: grid`, its children's (grid items) margin doesn't collapse.

## Some ways to avoid margin collapse
*Depends on the circumstances.*
- Padding in parent.
- Transparent border (don't forget `box-sizing: border-box`).
- Set child `display: inline-block`.

## Reference
[Mastering margin collapsing | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)
