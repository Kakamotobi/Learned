# Node JS
- A JavaScript runtime that executes code outside of the browser.
- Use the same syntax as JS to write server-side code.
- Notes
  - No window, document, DOM API.
  - Built-in modules that help us do things like interact with the operating system and files/folders.

## Node REPL in Terminal
- `node`
  - Open Node REPL.
- `node script.js`
  - Run Node files.
- `.exit`
  - Exit Node REPL.
- `global`
  - Top level scope containing the built-in functions, etc.
  - Equivalent of `window` in browser.

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
```
<greeter.js>
const args = process.argv.slice(2);
for (let arg of args) {
  console.log(`Hi there, ${arg}`);
}

<Terminal>
node greeter.js tom jerry spike
```

## File System Module
- Interacting with the file system (create/read/open/close/add to files/directories, etc.).
- *Need to require the module to use it:*
  - `const fs = require("fs");`
- *File system operations have synchronous and asynchronous (callback and promise-based) forms.*
### Example
```
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
