# Webpack

## Table of Contents
- [What is Webpack?](#what-is-webpack)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Webpack Configuration](#webpack-configuration)
    - [Configurations](#configurations)
    - [Example](#example)
    - [Multiple Entry Points Example](#multiple-entry-points-example)
  - [Entry File](#entry-file)
  - [Run Webpack](#run-webpack)
  - [Make Bundled Files Accessible by the Browser](#make-bundled-files-accessible-by-the-browser)
  - [Connect Bundled Files to HTML](#connect-bundled-files-to-html)
- 
- [Enhance Development Experience](#enhance-development-experience)
- [React, Webpack, Babel Setup](#react-webpack-babel-setup)

## What is Webpack?
> At its core, **webpack** is a *static module bundler* for modern JavaScript applications. When webpack processes your application, it internally builds a dependency graph from one or more *entry points* and then combines every module your project needs into one or more *bundles*, which are static assets to serve your content from. | webpack

- i.e. Webpack figures out all the dependencies among all the modules in the code base, including different assets (Ex: CSS, images, fonts), and compiles them into a few bundle of files that browsers can understand, and will download and execute.
  - For example, browsers do not understand SCSS. Therefore, SCSS files will have to be converted to regular CSS files.
- Usually used in frontend development. However, it can also be used in backend development to support SSR.
- Most libraries/frameworks will have their own webpack configuration so you will not have to use webpack directly.
  - It is "hidden" and you do not have to write Webpack files (for configuration).
### Some Alternatives to Webpack
#### Gulp
- A JavaScrript toolkit used as a streaming build system in front-end web development.
- Mainly used for automating tasks such as cache busting, linting, concatenation, minification, unit testing, optimization, etc.
- An easy alternative for Webpack but is less powerful.
#### Babel
- A JavaScript transpiler that converts ES6+ code to a previous version of JS that browsers can understand.
- It can also convert non-standard JS syntax such as JSX.

## Getting Started
### Installation
```zsh
npm i webpack webpack-cli -D
```
### Webpack Configuration
- Create a `webpack.config.js` file in the root directory.
- This file only understands old JS code.
#### Configurations
- **Entry Point** - required
  - The path to the file that webpack should process (the source code).
  - The "importer" of everything (JS and SCSS).
- **Mode**
  - `"development"` (default) or `"production"`.
- **Plugins**
  - While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks like bundle optimization, asset management and injection of environment variables.
  - Plugins have to be `require()`ed and added to the `plugins` array.
  - Plugins are customizable through options.
  - Plugins can be used multiple times in a configuration for different purposes.
    - Therefore, an instance has to be created (Ex: `new PluginName`).
- **Output** - required
  - The configuration for webpack's output.
  - `filename`: the filename for the bundled file.
  - `path`: the absolute path to the directory to put that file in.
    - `dir__name` returns the absolute path to the directory of the config file.
    - `path.resolve()` creates a whole path with as many segments/folders as you want.
  - `clean`: "cleans" the output folder before starting a new build (`npm run dev:assets`).
- **Rules**
  - Specifies the transformations for the different type of files.
  - `test`: the target files (using regex to find particular extensions).
  - `loader`: the transformation (found in node_modules by webpack) to be applied to the target files.
  - `options`: the particular options to be used for the loader.
#### Example
```js
// webpack.config.js

const path = require("path");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/client/js/main.js",
  mode: "development",
  watch: true,
  plugins: [new MiniCssExtractPlugin({ filename: "css/styles.css" })],
  output: {
    filename: "js/main.js",
    path: path.resolve(__dirname, "assets"),
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [["@babel/preset-env", { targets: "defaults" }]],
          },
        },
      },
      {
        test: /\.scss$/,
        // Loaders are in order of last to first to be executed.
        // SCSS --> CSS, CSS --> JS readable, Extract CSS into a separate file.
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
    ],
  },
};
```
- `style-loader` is usually used along with `css-loader`.
  - It injects the CSS (in the bundled JS file) to the DOM.
  - Therefore, JS will have to load to get the CSS on the screen.
- Instead, use `MiniCssExtractPlugin` to extract the CSS code into a separate file (`main.css`) that can be linked to in the markup.
  - Now, the `assets/js/main.js` file only handles JS.
#### Multiple Entry Points Example
```js
module.exports = {
  entry: {
    main: "./src/client/js/main.js",
    videoPlayer: "./src/client/js/main.js",
  },
  // ...
  output: {
    filename: "js/[name].js",
    // ...
  },
  // ...
}
```
### Entry File
```js
// src/client/js/main.js

import "../scss/styles.scss"; // Processed by sass-loader, css-loader, MiniCssExtractPlugin.loader.

console.log("This is the entry file!"); // Processed by babel-loader.
```
#### Other Files
- Files to be bundled and converted to more compatible versions.
- Put the bundled and converted version on the website. 
```scss
/* src/client/scss/styles.scss */

@import "./_variables.scss";

body {
  background-color: $red;
}
```
```scss
/* src/client/scss/_variables.scss */

$red: #ff3a3a;
```
### Run Webpack
```json
{
  "scripts": {
    "dev:assets": "webpack --config webpack.config.js",
  },
}
```
```zsh
npm run dev:assets
```
- Files are now bundled in the output directory.
  - `assets/css/styles.css`.
  - `assets/js/main.js`.
### Make Bundled Files Accessible by the Browser
```js
// Express Server

app.use("/static", express.static("assets"));
```
### Connect Bundled Files to HTML
```html
<head>
  <link rel="stylesheet" href="/static/css/styles.css"></link>
</head>
<body>
  <!--  ...  -->
  
  <script src="/static/js/main.js"></script>
</body>
```

## Enhance Development Experience
### Keep Changes Live and Updated
- **`watch: true`**
  - No need to execute `npm run dev:assets` after every change to frontend files.
    - Once executed, webpack will continue to watch and automatically build just like nodemon (for backend).
  - On a separate console, keep the server running.
- **`clean: true`** in Output
  - No need to manually delete the previous bundle before starting a new build.
```js
// webpack.config.js

module.exports = {
  // ...
  watch: true,
  output: {
    // ...
    clean: true,
  }
  // ...
}
```
### Seperate Changes to Backend and Frontend
- Prevent server from restarting after changes in frontend JS files.
#### nodemon.json
- Configure nodemon to ignore changes to webpack config file, frontend files and files in assets.
```json
{
  "ignore": ["webpack.config.js", "src/client/*", "assets/*"],
  "exec": "babel-node src/init.js",
}
```
#### package.json
- `nodemon` automatically looks for configs in `nodemon.json`.
- `webpack` automatically looks for configs in `webpack.config.js`.
  - No need for `"webpack --config webpack.config.js"`.
```json
{
  "scripts": {
    "dev:server": "nodemon",
    "dev:assets": "webpack",
  },
}
```

## React, Webpack, Babel Setup
### Webpack
#### Entry/Ouput
```js
const path = require("path");

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
  }
}
```
#### Add Bundled JS file to HTML
- There are two ways to do this.
##### Manual
- Insert the script tag in the body of the html file.
##### Plugin
- The `html-webpack-plugin` takes the `/public/index.html` and inject the (bundled JS file's) script tag to it.
- Then, it moves that HTML file to the output directory `dist`.
```js
// ...
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "src", "index.html"),
    }),
  ],
}
```
#### Incorporate Babel into Webpack
```js
module.exports= {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env", "@babel/preset-react"],
          },
        },
      },
    ]
  },
  // ...
}
```
### Build and Run
- **`webpack serve`**
  - Uses the webpack dev server to run the application locally.
- **`webpack`**
  - Create a bundle of assets that is deployable to servers.
```json
{
  "scripts": {
    "dev": "webpack serve",
    "build": "webpack",
  }
}
```

## Reference
[Concepts | webpack](https://webpack.js.org/concepts/)  
[nodemon documentation](https://github.com/remy/nodemon#config-files)  
[Setup react with webpack and babel | by Prateek Srivastava | Age of Awareness | Medium](https://medium.com/age-of-awareness/setup-react-with-webpack-and-babel-5114a14a47e9)  
