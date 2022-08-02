# TypeScript Compilation and Configurations

## Table of Contents
- [Compilation](#compilation)
- [Configurations](#configurations)
  - [Create `tsconfig.json` File](#create-tsconfigjson-file)
  - [Specify Files to Compile](#specify-files-to-compile)
    - [Option 1 - `files`](#option-1---files)
    - [Option 2 - `include` and `exclude`](#option-2---include-and-exclude)
  - [`outDir` Option](#outdir-option)
  - [`target` Option](#target-option)
  - [`strict` Option](#strict-option)

## Compilation
### Compile TS to JS
```zsh
tsc filename.ts
```
#### Start Compilation in Watch Mode
```zsh
tsc -w filname.ts
```
### Compiling Multiple TS Files
- The generic `tsc` command compiles any/all TS files in the directory.


## Configurations
### Create `tsconfig.json` File
```zsh
tsc --init
```
### Specify Files to Compile
#### Option 1 - `files`
- A top-level option where files to include in the program are listed.
```json
{
  "compilerOptions": {},
  "files": [
    "index.ts",
    "main.ts"
  ]
}
```
#### Option 2 - `include` and `exclude`
- `include`
  - A top-level option to specify an array of filenames or patterns to include in the program.
  - If `files` option is specified, the `include` option will not activate.
  - Defaults to everything (`**`).
- `exclude`
  - A top-level option to specify an array of filenames or patterns to exclude from the program.
  - Default value is `node_modules`.
    - Therefore, if specifying `exclude`, make sure to exclude `node_modules` as well to avoid compiling dependencies.
- Example
  - `include` everything in `src` folder but `exclude` the `dontCompileThis.ts` file.
  ```json
  {
    "compilerOptions": {},
    "include": ["src"],
    "exclude": ["src/dontCompileThis.ts", "node_modules"]
  }
  ```
### `outDir` Option
- Specify where TS should emit the compiled JS files.
- Defaults to the location of the corresponding TS files.
- Example
  - Compile all TS files in the `src` folder into the `dist` folder.
```json
{
  "compilerOptions": {
    "outDir": "./dist"
  },
  "include": ["src"]
}
```
### `target` Option
- Set the JS version to compile to.
- Defaults to ES3.
### `strict` Option
- Enables all strict type-checking options.
- Defaults to `true`.
