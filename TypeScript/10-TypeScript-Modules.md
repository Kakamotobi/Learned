# TypeScript Modules

## Table of Contents
- [Writing Module-based Code in TypeScript](#writing-module-based-code-in-typescript)
- [Changing the Compilation Module System](#changing-the-compilation-module-system)
- [Importing Types](#importing-types)

## Writing Module-based Code in TypeScript
> Before we start, it’s important to understand what TypeScript considers a module. The JavaScript specification declares that any JavaScript files without an `export` or top-level `await` should be considered a script and not a module. | TypeScript

> Inside a script file variables and types are declared to be in the shared global scope, and it’s assumed that you’ll either use the outFile compiler option to join multiple input files into one output file, or use multiple `<script>` tags in your HTML to load these files (in the correct order!). | TypeScript

> If you have a file that doesn’t currently have any imports or exports, but you want to be treated as a module, add the line: | TypeScript
> ```ts
> export {}
> ```

- After compiling TS into JS,
  - files that do not have an `export` or top-level `await`, are considered script files and not modules.
  - Therefore, variables and types in the script files are shared in the global scope (namespace).
    - This means that they have to be manually included in the correct order in the HTML.
    - No module syntax (CJS or ESM, etc.).
- TypeScript supports ES Modules syntax (`import`/`export`).
- *TS is able to compile the code to some other module syntax.*
  - Ex: you can write code in ESM and output can be in CJS (or choose to stay ESM).
### Example
```ts
// index.ts

import { add, sample } from "./utils.js";

add(1, 2);
sample([1, 2, 3]);
```
```ts
// utils.ts

export function add(x: number, y: number) {
  return x + y;
}

export function sample<T>(arr: T[]): T {
  const idx = Math.floor(Math.random() * arr.length);
  return arr[idx];
}
```

## Changing the Compilation Module System
- JS in the browser does not know about CommonJS modules (`require`, `exports`).
  - Therefore, we can tell TS to compile to ES modules instead (or use script files instead of modules).
    - Ex: `"module": "ES6"` in `tsconfig.json`.
  - However this by itself leads to an error: `"Uncaught Syntax Error: Cannot use import statement outside of a module"`.
    - Therefore, the `type="module"` attribute has to be included in the `script` tag in the HTML.
    - *Modules need to be served (does not work on file protocol) for example, via `lite-server`.*
### Example - `tsconfig.json`
```json
{
  "module": "ES6"
}
```

## Importing Types
- Types only exist in TypeScript so they "disappear" when TS is compiled to JS.
  - So can they be exported and imported?
- When importing types, esp. through other transpilers like Babel, you may run into issues.
  - Since TypeScript version 3.8, `type` can be included in `import` statements for types.
    - `import type` is always guaranteed to be removed from JS.
### Example
```ts
// types.ts

export interface Person {
  username: string;
  email: string;
}

export const notAType = "I am not a type";
```
```ts
// User.ts

import type { Person } from "./types.js";
// OR
// import { type Person, notAType } from "./types.js";

export default class User implements Person {
  constructor(public: username: string, public email: string) {}
  
  logout() {
    console.log(`${this.username} logged out!`);
  }
}
```

## Reference
[TypeScript: Documentation - Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html)  
