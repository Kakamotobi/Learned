# Syntactically Awesome Style Sheets (Sass)

## Table of Contents
- [What is Sass?](#what-is-sass)
- [Sass Features](#sass-features)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Ways to Convert Sass to CSS](#ways-to-convert-sass-to-css)

## What is Sass?
> Sass is a stylesheet language that's compiled to CSS. | Sass

- A CSS extension language that helps you write robust, maintainable CSS.
  - Provides features that do not yet exist in CSS such as nesting, mixins, inheritance, etc.
- Sass is simply the syntax and definition.
- **Dart Sass** and **Node Sass** are its implementations.
- Browsers do not understand Sass. Therefore, it has to be converted to normal CSS.
### `.sass` vs `.scss`
- There are two syntaxes for Sass: indented syntax (`.sass`) and the SCSS syntax (`.scss`).
#### `.sass`
- Uses indentation instead of curly braces to nest statements.
- Uses new lines instead of semicolons to separate lines.
#### `.scss`
- A superset of CSS (all valid CSS is valid in SCSS).
- More commonly used.

## Sass Features
### Variables
```scss
$main-font: Helvetica, sans-serif;
$primary-text-color: #000;

body {
  font-family: $main-font;
  color: $primary-text-color;
}
```
### Nesting
```scss
nav {
  display: flex;
  
  ul {
    padding: 20px;
    list-style: none;
  }
  
  li {
    display: inline-block;
    
    a {
      text-decoration: none;
    }
  }
}
```
### Partials
- Create partial Sass files containing snippets of styles that can be included in other Sass files.
- Allows you to modularize CSS and help keep things easier to maintain.
- Sass partial files are named with a leading underscore (Ex: `_partial.scss`).
  - The underscore indicates to Sass that it is only a partial file, and hence should not be generated into a CSS file.
- **`@use`**
  - Used to load another Sass file as a module in another Sass file. 
    - These will also be included in the compiled output.
  - Hence, can use its variables, mixins, and functions.
- Example
  ```scss
  // _colors.scss
  
  $primary-text-color: #000;
  $secondary-text-color: #222;
  ```
  ```scss
  // styles.scss
  
  @use "colors";
  
  body {
    color: colors.$primary-text-color;
    
    a {
      color: colors$secondary-text-color;
    }
  }
  ```

## Getting Started
### Installation
#### Dart Sass Implementation
- The primary implentation of Sass.
- Compiles to pure JS (hence, easy integration).
- Requires a native library installed.
- OS
  ```zsh
  brew install sass/sass/sass
  ```
#### Node Sass Implemetation
- Pure JS implmentation of Sass.
- A bit slower than Dart Sass.
- Global
  ```zsh
  npm install -g sass
  ```
- Project
  ```zsh
  npm install sass -D
  ```
### Ways to Convert Sass to CSS
#### Terminal
- Once Sass is installed, Sass can be compiled into CSS by using the `sass` command.
- Tell Sass which file to build from, and where to output CSS to.
- **`--watch`** flag
  - Watch individual files or directories for changes and recompile CSS each time the Sass file is saved.
- Examples
  - Watch and take the `input.scss` file and compiles that file to `output.css`.
    ```zsh
    sass --watch input.scss output.css
    ```
  - Watch the files in the `app/sass` directory and compile them to the `public/stylesheets` directory.
    ```zsh
    sass --watch app/sass:public/stylesheets
    ```
#### Webpack
```js
// webpack.config.js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  // ...
  plugins: [new MiniCssExtractPlugin({ filename: "css/styles.css" })],
  // ...
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
    ],
  },
};
```

## Reference
[Sass: Syntactically Awesome Style Sheets](https://sass-lang.com/)  
