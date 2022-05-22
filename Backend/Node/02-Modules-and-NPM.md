# Modules and NPM

## Table of Contents
- [Node Modules](#node-modules)
- [Node Package Manager (NPM)](#node-package-manager-npm)
- [package.json](#packagejson)
- [Example - Language Guesser](#example---language-guesser)

## Node Modules
- By default, we cannot share variables, etc. between files.
- Hence, we need to use Node Modules.
- **Files get required just once in the lifecycle of a project.**
  - If file is required again, we get whatever is inside of the *Require Cache*.
  
### `module.exports` and `require("")`
#### `module.exports` (shorthand: `exports`)
- An empty object that we add methods and properties onto that we then explicitly export.
- We need to explicitly indicate what a file can share and hence, what other files can `require("")`.
#### `require("")`
- Returns the `exports` object from another file/directory.
- *For Node modules, when a file is created, the content of the file is not automatically available everywhere else when the file is `require`d*.
- `console.log(require.cache)`
  - Object that stores the result of requiring in any file to our project.
#### Example
```js
// mathFormulas.js

const PI = 3.14159;
const square = (x) => x * x;

// Saving properties and functions onto the exports object.
exports.PI = PI;
exports.square = square;
```
```js
// app.js

// Importing exports object from mathFormulas.js.
// ./ refers to current directory.
const mathFormulas = require("./mathFormulas.js");

// OR we can destructure as well.
const { PI, square } = require("./mathFormulas");
```
```zsh
node app.js
```
#### `require("")` an Entire Directory
- Node looks for an index.js file and whatever the file exports is what the directory will export.

## Node Package Manager (NPM)
- The default package manager for JavaScript that maintains the installation of packages (libraries).
- A library (standardized repository) of thousands of packages published by other developers that we can use for free.
  - Ex: package for React, package for Express, package for making rainbow text, etc.
- A command line tool that we can use to easily install and manage those packages in our Node projects.
### Clarifications
- Yarn - another JavaScript package manager.
- Homebrew - a package management system that maintains the installation of software on maxOS and Linux.
- NPX
  - An npm package runner that was introduced from npm@5.2.0.
  - The same way npm makes it easy to install and manage dependencies hosted on the registry, npx makes it easy to use CLI tools and other executables hosted on the registry.
  - Unlike NPM, which installs packages either globally or locally, NPX installs the package on your local machine while executing the package but then proceeds to delete it.
### Installing Packages (in the current directory)
```zsh
npm install packageName
```
- Downloads a folder called **"node_modules"**, which contain all the **dependencies** for that package.
- Also downloads a file called **"package-lock.json"**, which is a record of the contents of the **"node_modules"** directory.
- *Need to `require("")` the package name (not file path) to use it.*
#### Example
```zsh
npm install give-me-a-joke
```
```js
// index.js

const jokes = require("give-me-a-joke");
// from the package docs
jokes.getRandomDadJoke (function(joke) {
  console.log(joke);
});
```
```zsh
node index.js
```
### Local vs. Global Installation of Packages
- Cannot `require("")` a module using its package name if it is installed locally into another directory.
- `-g`
  - Install a package globally.
  - *Usually tools that give us new command line commands.*
  - Ex: `npm install -g cowsay`
- `npm link globalPackageName`
  - Creates a symlink in the global folder that links to the package where the `npm link` command was executed.

## package.json
- File that contains meta data about the particular project/package/application.
  - Ex: description, license, name, version, dependencies.
- Acts as a record of everything that is being used in the applicaiton.
- `npm init`
  - Creation utility for package.json file.
  - After creating a package.json file, packages that are installed henceforth are automatically recorded in the file, under dependencies.
- ***Make sure to keep this record in sync with the "node_modules" folder.***
- *Typically placed in the root directory of a project.*
### Using the package.json File to Install All Packages/Dependencies
- **`npm install`** inside the root directory containing the package.json file of the project.
  - Installs the **"node_modules"** directory.
### package.json `"scripts"`
```json
"scripts": {
  "win": "node index.js"
}
```
```zsh
npm run win # executes `node index.js`
```
### dependencies vs. devDependencies
- dependencies include libraries, etc. necessary for the application.
  - Ex: express, react. 
- devDependencies include things that developers need.
  - Ex: babel, nodemon.
### package-lock.json
> package-lock.json is automatically generated for any operations where npm modifies either the node_modules tree, or package.json.  
> It describes the exact tree that was generated, such that subsequent installs are able to generate identical trees, regardless of intermediate dependency updates.
- ***Describes a single representation of a dependency tree such that teammates, deployments, and continuous integration are guaranteed to install exactly the same dependencies.***
  - i.e., it ensures that the version numbers are the same on everything (dependency versions, etc.).

## Example - Language Guesser
```zsh
npm install franc
npm install langs
npm install colors
```
```js
// index.js

const franc = require("franc");
const langs = require("langs");
const colors = require("colors");

const text = process.argv[2];

try {
  console.log(langs.where("3", franc(text)).name.green);
} catch (err) {
  console.log("Could not match a language. Please try again with a longer sample".red);
}
```
```zsh
node index.js "Bonjour, je m'appelle Etienne" # returns French
```

## Reference
[npm Docs](docs.npmjs.com)
