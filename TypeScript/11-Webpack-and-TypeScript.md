# Webpack and TypeScript

## Table of Contents
- [Webpack](#webpack)
- [Setting Up Webpack with TypeScript](#setting-up-webpack-with-typescript)
- [Source Maps](#source-maps)
- [Webpack for Development](#webpack-for-development)
- [Webpack for Production](#webpack-for-production)
  - [`clean-webpack-plugin`](#clean-webpack-plugin)

## Webpack
- Websites, with the introduction of SPAs, are not pre-built anymore for the most part. Instead, the browser makes requests to load files.
  - First request starts of with HTML, CSS, and maybe some JS. Henceforth, it sends requests to load more JS, etc.
- Webpack aims to find the balance between the following:
  - **Preventing bottleneck caused by numerous requests.**
    - Each requests take time (i.e. each script requires a new HTTP request by the browser).
    - Therefore, having numerous script files will lead to that many number of requests.
  - **Writing code in an organized way, which requires multiple scripts.**
    - Jamming all code into very few number of files is terrible for developers.
    - Therefore, we "need" to have a lot of scripts.
- Webpack takes JS, SASS, image files, etc. and bundles them together into static assets.
  - Ex: hundreds of JS files with each respective dependent libraries and bundle them into a file(s), which can then be loaded as a single script.
- More on Webpack [here](https://github.com/Kakamotobi/Learned/blob/main/Tools/Webpack.md).

## Setting Up Webpack with TypeScript
```zhs
npm i -D webpack webpack-cli ts-loader typescript
```
- `ts-loader` is the middleman between Webpack and Typescript.
  - It basically calls the `tsc` command and comiles TS into JS, then hand it over to Webpack, which bundles everything together.
### Configuration
```js
// webpack.config.js

const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/
      },
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
}
```

## Source Maps
- Source Maps help us debug and make sense of the minified bundled code.
  - Source Maps take the minified code and map it backwards to its pre-built state so that we can see where a particular code is actually coming from.
- In the browser's "Sources" tab, there is a "webpack_ts" dropdown where you can see the files.
### Setup
```json
// tsconfig.json

{
  "compilerOptions": {
    "sourceMap": true
  }
}
```
```js
// webpack.config.js

module.exports = {
  devtool: "inline-source-map", // extract source maps and include them in the final bundle.
}
```

## Webpack for Development
- In `"development"` mode, the bundled file is not minified.
  - cf. in `"production"` mode, the bundled file is minified.
```js
// webpack.config.js

module.exports = {
  mode: "development",
}
```
### Webpack Dev Server
- The Webpack Dev Server is a live server that also handles the bundling behind the scenes (without having to manually run the `webpack` command/script).
  - It keeps the bundle in memory (in `"development"` mode), instead of making a separate bundle file every single time and writing to disk.

```zsh
npm i -D webpack-dev-server
```
```json
// package.json

{
  "scripts": {
    "serve": "webpack serve"
  }
}
```
```js
// webpack.config.js

module.exports = {
  devServer: {
    static: {
      directory: path.join(__dirname, "./"),
    },
  },
  output: {
    // ...
    publicPath: "/dist",
  }
}
```

## Webpack for Production
- There are a few ways to go about setting up Webpack for production.
- Example
  - Separate config files for development and production.
  ```js
  // webpack.dev.config.js
  
  module.exports = {
    mode: "development",
  }
  ```
  ```js
  // webpack.prod.config.js
  
  module.exports = {
    mode: "production",
  }
  ```
  ```json
  // package.json
  
  {
    "scripts": {
      "serve": "webpack serve --config webpack.dev.config.js", // bundle file in memory for development.
      "build": "webpack --config webpack.prod.config.js" // build bundle file for production.
    }
  }
  ```
### `clean-webpack-plugin`
- It is common to include some form of a hash into the output file name.
  - This helps the browser recognize that changes have been made, effectively avoiding possible issues with caching, etc.
  - Ex: `"[contenthash].bundle.js"`.
    - Webpack automatically adds a hash based on the contents of the bundle file.
- However, doing this will end up stacking up bundled files.
- Therefore, use the `clean-webpack-plugin` to automatically empty out the `dist` folder everytime the bundle is rebuilt.
- *Note*
  - Your HTML does not know about the hash in the name of the bundle file. So, a possible solution is to use Webpack to build the HTML for you.
#### Setup
```zsh
npm i -D clean-webpack-plugin
```
```js
// webpack.prod.config.js

import { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
  plugins: [
    new CleanWebpackPlugin(),
  ],
}
```




