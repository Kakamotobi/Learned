# Babel

## Table of Contents
- [What is Babel?](#what-is-babel)
- [Getting Started](#getting-started)
- [Node.js Development Setup Example (ft. Nodemon)](#nodejs-development-setup-example-ft-nodemon)
- [Node.js Production Setup Example](#nodejs-production-setup-example)

## What is Babel?
> Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments. | Babel

- Babel is a JS compiler that allows the use of new syntax without having to wait for browser support.
- It can also convert other syntaxes.
  - Use the React preset (`@babel/preset-react`) for JSX syntax.
  - Use the TypeScript preset (`@babel/preset-typescript`) to strip out type annotation.

## Getting Started
### Installation
#### Install Babel
- This consists of the core functionality of Babel.
  - It is responsible for parsing and output. Plugins/presets are responsible for teh actualy conversion.
- It also contains `core-js/stable`, the module which is used to polyfill EcmaScript features.
  - Polyfills are pieces of code to provide modern functionality on older browsers that do not natively support it.
  - It does this by updating or adding missing implementations.
```zsh
npm install --save-dev @babel/core
```
#### Install CLI
##### Babel CLI
- Used to compile files from the command line.
```zsh
npm install --save-dev  @babel/cli
```
##### Node.js Babel CLI
- `babel-node` is not meant for production as it is exhaustive.
```zsh
npm install --save-dev @babel/node
```
#### Install Preset or Plugin for Specific Syntax
- Presets are a pre-determined set of plugins(transformations).
##### ES2015+ Syntax
```zsh
npm install --save-dev @babel/preset-env
```
##### React
```zsh
npm install --save-dev @babel/preset-react
```
##### TypeScript
```zsh
npm install --save-dev @babel/preset-typescript
```
### Run Babel
- Compile all source code in the `src` directory to `lib` using the `@babel/env` preset.
```zsh
./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
```
- Or
```zsh
npx babel src --out-dir lib --presets=@babel/env
```

## Node.js Development Setup Example (ft. Nodemon)
### Installation
```zsh
npm install @babel/core @babel/node @babel/preset-env
npm install nodemon
```
### `babel.config.json`
```json
{
  "presets": ["@babel/preset-env"]
}
```
### `nodemon.json`
```json
{
	"exec": "babel-node src/init.js"
}
```
### `package.json`
```json
{
  "scripts": {
    "dev": "nodemon -exec babel-node src/server.js"
  }
}
```
### Run
```zsh
npm run dev
```

## Node.js Production Setup Example
### Installation
```zsh
npm install @babel/core @babel/cli @babel/preset-env
```
### `babel.config.json`
```js
{
  "presets": ["@babel/preset-env"]
}
```
### `package.json`
```json
{
  "scripts": {
    "build": "babel src -d build"
  }
}
```
### Run
```zsh
npm run build
```

## Reference
[What is Babel? · Babel](https://babeljs.io/docs/en/)  
[Usage Guide · Babel](https://babeljs.io/docs/en/usage)  
[@babel/cli · Babel](https://babeljs.io/docs/en/babel-cli)  
