# Modules and NPM

## Node Modules
### `module.exports` and `require("")`
#### `module.exports` (shorthand: `exports`)
- An empty object that we add methods and properties onto that we then explicitly export.
- We need to explicitly indicate what a file can share and hence, what other files can `require("")`.
#### `require("")`
- Returns the `exports` object from another file/directory.
- *For Node modules, when a file is created, the content of the file is not automatically available everywhere else when the file is `require`d*.
#### Example
```
<mathFormulas.js>
const PI = 3.14159;
const square = (x) => x * x;

// Saving properties and functions onto the exports object.
exports.PI = PI;
exports.square = square;

<app.js>
// Importing exports object from mathFormulas.js.
// No need to include ".js".
// ./ refers to current directory.
const mathFormulas = require("./mathFormulas");

// OR we can destructure as well.
const { PI, square } = require("./mathFormulas");

<Terminal>
node app.js
```
#### `require("")` an Entire Directory
- Node looks for an index.js file and whatever the file exports is what the directory will export.

## Node Package Manager (NPM)
1. A library (standardized repository) of thousands of packages published by other developers that we can use for free.
  - Ex: package for React, package for Express, package for making rainbow text, etc.
2. Comes with a command line tool that we can use to easily install and manage those packages in our Node projects.
### Installing Packages (in the current directory)
```
<Terminal>
npm install packageName
```
- Downloads a folder called **"node_modules"**, which contain all the **dependencies** for that package.
- Also downloads a file called **"package-lock.json"**, which is a record of the contents of the **"node_modules"** directory.
- *Need to `require("")` the package name (not file path) to use it.*
#### Example
```
<Terminal>
npm install give-me-a-joke

<index.js>
const jokes = require("give-me-a-joke");
// from the package docs
jokes.getRandomDadJoke (function(joke) {
  console.log(joke);
});

<Terminal>
node index.js
```
## Local vs. Global Installation of Packages
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
- `npm install` inside the root directory containing the package.json file of the project.
  - Installs the **"node_modules"** directory.
### package-lock.json
> package-lock.json is automatically generated for any operations where npm modifies either the node_modules tree, or package.json.  
> It describes the exact tree that was generated, such that subsequent installs are able to generate identical trees, regardless of intermediate dependency updates.
- *Importance:*
  - *Describes a single representation of a dependency tree such that teammates, deployments, and continuous integration are guaranteed to install exactly the same dependencies.*
## Reference
[npm Docs](docs.npmjs.com)
