# Intro to TypeScript

## Table of Contents
- [What is it?](#what-is-it)
- [Why use it?](#why-use-it)
- [Installation](#installation)
- [Compiling TS to JS](#compioling-ts-to-js)
- [Other Notes](#other-notes)

## What is it?
> TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale. | TypeScript

### Two Major Parts
- TypeScript provides additional syntax (types) and performs **static checking**. Therefore, we can catch errors early on before runtime/execution, in development.
- TypeScript compiles to JavaScript. Therefore, it can run in any JavaScript runtime environment.

## Why use it?
- JavaScript is a dynamic and weakly typed language.
  - Therefore, values of different types may not lead to errors and instead return falsy values.
- This can potentially cause unexpected issues in the codebase.
- Examples
  ```js
  null * 213; // 0
  ```
  ```js
  undefined * 8; // NaN
  ```
  ```js
  const user = { name: "Tom", age: 3 };
  user.gender; // undefined
  ```
- TypeScript helps us avoid these pitfalls and bugs that arise in JavaScript.

## Installation
### Per Project Basis
```zsh
npm i -D typescript
```
### Global Installation
- Provides the use of `tsc` command anywhere in terminal.
```zsh
npm i -g typescript
```

## Compiling TS to JS
- This creates the same TS file but compiled to JS.
```zsh
tsc index.ts
```

## Other Notes
- `.ts` refers to TS files.
- `.tsx` refers to TS + JSX files.

## Reference
[TypeScript: JavaScript With Syntax for Types.](https://www.typescriptlang.org/)  
