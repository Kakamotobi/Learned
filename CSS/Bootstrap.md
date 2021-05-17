# Bootstrap
- CSS framework that gives us access to pre-built components and functionalities that we can incorporate into our own website.
- Most features are enacted through classes.

## Including Bootstrap
- Download the physical Bootstrap stylesheets and link to it.
- Use a CDN.
  - A hosted version of a stylesheet that can be accessed remotely (no need to download).
  - Include CSS link and JS link (for certain components that require JS. Ex: image carousel, collapsible navbar).

## Components
- Ex: alerts, navbar, dropdowns, carousel.
### Buttons
- Ex: `class="btn btn-primary"`

### Forms
- `class="form-group"`
  - Group a section of the form together.
- `class="form-control"`
  - Put on the input, select, etc. itself.
#### Example
```
<div class="container">
  <h1 class="display-2">Forms</h1>
  <form action="nowhere">
    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" class="form-control" id="email" placeholder="email">
    </div>
    <div class="form-group">
      <label for="password">Password</label>
      <input type="password" class="form-control" id="password" placeholder="password">
    </div>
    <div class="form-group">
      <label for="state">State</label>
      <select name="state" class="form-control" id="state" placeholder="state">
        <option value="AL">Alabama</option>
        <option value="AK">Alaska</option>
      </select>
    </div>
  </form>
```
#### Form Layout with Grid System
- `class="form-row"` and `class="form-group col"`
##### Example
```
<div class="container">
  <h1 class="display-2">Forms</h1>
  <form action="nowhere">
    <div class="form-row">
      <div class="form-group col">
        <label for="email">Email</label>
        <input type="email" class="form-control" id="email" placeholder="email">
      </div>
      <div class="form-group col">
        <label for="password">Password</label>
        <input type="password" class="form-control" id="password" placeholder="password">
    </div>
  </form>
```

## Content - Typography
- Display Headings
  - When you need a heading to stand out.
  - Not responsive by default, but it's possible to enable responsive font sizes.
- Text Alignment
  - Ex: `class="text-center"`

## Layout - Grid System
- Helps us construct our own custom, responsive layouts.
- *Only works inside of a container.*
- Container
  - `class="container"`
    - The most basic layout element in Bootstrap.
    - Containers are used to contain, pad, and (sometimes) center the content within them.
  - `class="container-fluid"`
    - A full width container, spanning the entire width of the viewport.
- Also need to create a row.
  - `class="row"`
  - Every row has 12 units of space to divide (column-wise).
- Create columns in each row.
  - `class="col"`
  - Ex: `class="col-6"` allocate 6 units.

### Example
```
<div class="container">
  <div class="row">
    <div class = "col-6"></div>
    <div class = "col-6"></div>
  </div>
</div>
```
### Responsive Layouts using Grid System
- Use built-in breakpoints (pre-defined viewport width) in Bootstrap.
- Ex: `class="col-md-6 col-xl-3"`
  - Everything below the medium breakpoint will take up the full width across.
  - After the medium breakpoint and up, take up 6 units.
  - After the extra-large breakpoint, take up 3 units.

### Responsive Images
- `class="img-fluid"`
  - Makes an image scale based upon the size of its containing element.

### Grid Utilities
- Utilities have different variants for different breakpoints.
  - Ex: `class="justify-content-md-center"`

## Other Useful Utilities
### Spacing
- Format for xs: `{property}{sides}-{size}`
- Format for sm, md, lg, xl: `{property}{sides}-{breakpoint}-{size}`
- Ex: `mt-md-3`
### Display
### Sizing

## Bootstrap Icons
- SVG elements.

## Reference
[Bootstrap](https://getbootstrap.com/)
