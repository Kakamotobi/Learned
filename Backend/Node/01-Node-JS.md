# Node JS
- **A JavaScript runtime environment that executes outside of the browser (runs directly on a computer or server OS), and allows us to create web servers, command line tools, native apps (ex: VSCode), etc.**
- Thanks to Node JS, we have things like React Native (build mobile apps), Electron (build desktop apps), etc.
- Notes
  - No window, document, DOM API.
  - Have built-in modules that help us do things like interact with the operating system and files/folders.

## Table of Contents
- [Brief History](#brief-history)
- [Node REPL in Terminal](#node-repl-in-terminal)
- [Debugging Node JS](#debugging-node-js)
- [CLI Debugger Commands](#cli-debugger-commands)
- [`process` and `process.argv`](#process-and-processargv)
- [File System Module](#file-system-module)

## Brief History
- JavaScript used to only be in the browser. However, Ryan Dahl took JavaScript outside of the browser and made it possible for it to be an ordinary general-purpose programming language such as Python, Java, C.
- Hence, it became possible to use JavaScript to make anything you want including backend, write scripts to rename files, upload files, process images, etc.
- With the birth of Node JS, JavaScript was no longer limited to the browser.

## Node REPL in Terminal
- `node`
  - Open Node REPL.
- `node script.js`
  - Run Node file.
- `.exit` or "ctrl + d" or "ctrl + c" twice
  - Exit Node REPL.
- `global`
  - Top level scope containing the built-in functions, etc.
  - Equivalent of `window` in browser.

## Debugging Node JS
- `node inspect filename.js`
  - Start up a debugger CLI and pause execution whenever a 'debugger' statement is hit.
- `node --inspect filename.js`
  - Start up a debugger instance and pause execution whenever a 'debugger' statement is hit.
  - Access the debugger at 'chrome://inspect'.
- `node --inspect-brk filename.js`
  - Start up a debugger instance and *wait to execute* until a debugger is connected.
  - Access the debugger at 'chrome://inspect'.

## CLI Debugger Commands
- `c`
  - Continue execution until program ends or next debugger statement.
- `n`
  - Run the next line of code.
- `s`
  - Step into a function.
- `o`
  - Step out of the current function.
- `repl`
  - Start up an execution environment where we can reference the different variables inside of our program.

## `process` and `process.argv`
### `process`
- Object that provides information about, and control over, the current Node.js process.
  - Methods, properties.
  - Ex: `process.cwd()` // path to where Node is currently running in.
- Available in the `global` scope.
### `process.argv`
- Returns an array containing the command line arguments passed when the Node.js process was launched.
  - First Element: `process.execPath`.
    - Absolute pathname of the executable that started in the Node.js process.
  - Second Element: the file that we are executing.
  - Any remaining elements will be additional command line arguments.
#### Example
```js
// greeter.js

const args = process.argv.slice(2);
for (let arg of args) {
  console.log(`Hi there, ${arg}`);
}
```
```zsh
// Terminal

node greeter.js tom jerry spike
```

## File System Module
- Interacting with the file system (create/read/open/close/add to files/directories, etc.).
- *Need to require the module to use it:*
  - `const fs = require("fs");`
- *File system operations have synchronous and asynchronous (callback and promise-based) forms.*
### Example
```js
// boilerplate.js

const fs = require("fs");

// Make first argument the folder name (default: Project).
const folderName = process.argv[2] || "Project";

try {
  fs.mkdirSync(folderName);
  fs.writeFileSync(`${folderName}/index.html`, "");
  fs.writeFileSync(`${folderName}/style.css`, "");
  fs.writeFileSync(`${folderName}/app.js`, "");
} catch (err) {
  console.log(err);
}
```

## Reference
[Index | Node.js v14.16.1 Documentation](https://nodejs.org/dist/latest-v14.x/docs/api/)  
[Express/Node introduction - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/Introduction)
