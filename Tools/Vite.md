# Vite

## Table of Contents
- [What is Vite?](#what-is-vite)
- [Getting Started](#getting-started)
- [Notes](#notes)
- [Reference](#reference)

## What is Vite?
> ... a build tool that aims to provide a faster and leaner development experience for modern web projects.

- Vite leverages native ES Modules in the browser to load code instantly no matter the size of the application.
- It also supports Hot Module Reloading (HMR) for development.
  - HMR: changes in the code are reflected instantly without losing the state of the application.
- When building for production, it uses Rollup under the hood. Therefore, no need to worry about configuring it.
- Vite, as do other module bundlers, runs in your computer's Node.js.
  - i.e. a Node-based process does the transformation.

### Two Parts to Vite
1. Provide a local development server.
2. Bundle JavaScript, CSS and other assets together for production.

## Getting Started
### Installation
```zsh
npm init vite app-name
```
- Then, select the framework of your choosing (Ex: React, Svelte, Vanilla).
### Vite Configuration
- `vite.config.js` file is created upon installation.
- Vite configuration has a plugin ecosystem that can extend it with additional features.
  - Plugin Ex: `vite-plugin-ssr`.
- You can also manually override the Rollup defaults when necessary.
### Serve Locally
```zsh
npm run dev
```
- Even with a lot of dependencies, the time to run the dev server does not change.
- Instead of importing a single JS bundle file, it imports the actual source code.
  - Check the "Network" tab in dev tools.
### Build for Production
```zsh
npm run build
```
- Creates a `dist` folder containing the source code that has been bundled by Rollup.
- Optimizations are automatically added.
  - Ex: automatic code-splitting for dynamic imports, CSS, etc.

## Notes
### TypeScript
  - Vite does not perform type checking for TypeScript files. It only transpiles them because it assumes type checking is taken care of by the used IDE (in development) and build process (for production).
  - It uses esbuild to transpile TypeScript code to JavaScript.

## Reference
[Getting Started | Vite](https://vitejs.dev/guide/#overview)  
