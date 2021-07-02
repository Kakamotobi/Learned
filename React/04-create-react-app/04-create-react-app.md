# Create React App
- A utility script that:
  - Creates a a skeleton react project with files and folders already created.
    - The "src" folder is where all the components we make go in.
    - The "public" folder is where the index.html file is in.
  - Sets it so that JS files are run through Babel automatically.
  - Lets us use super-modern JavaScript features/idioms.
  - Makes testing and deployment much easier.
- *Use this instead of using CDN Links.*

## Table of Contents
- [Installing Create React App](#installing-create-react-app)
- [Terminal Commands in New Directory](#terminal-commands-in-new-directory)
- [Webpack](#webpack)
- [Modules](#modules)
- [CRA Conventions for Components](#cra-conventions-for-components)
- [CSS and Assets in CRA](#css-and-assets-in-cra)

## Installing Create React App
### Installation Method 1 - using NPX
- Only works with npm 5.2 or higher.
- No need to install `create-react-app` first.
- Create a new `create-react-app` instance with the specified name.
```
npx create-react-app name-of-your-app
cd name-of-your-app
npm start
```
### Installation Method - using NPM
- Install globally just once.
- Henceforth, do **`create-react-app name-of-your-app`** to create each React project that you want.
```
npm install -gcreate-react-app
create-react-app name-of-your-app
cd name-of-your-app
npm start
```

## Terminal Commands in New Directory
- **`npm start`**
  - Starts the development server for your app.
  - Execute inside of the application directory.
  - Automatically open up in browser.
- **ctrl + c**
  - Stop the server running.
- **`npm run build`**
  - Bundles the app into static files for production.
- **`npm test`**
  - Starts the test runner.
- **`npm run eject`**
  - Removes this tool and copies build dependencies, configuration files and scripts into the app directory. If you do this, you can't go back!

## Webpack
- CRA is built on top of "Webpack", which is a JS utility that:
  - Enables us to import/export modules.
    - Makes it easier for us to reference files and assets in other files.
    - Packages up all CSS, images, JS into a single file for browser.
    - Dramatically reduces the number of HTTP requests that we have to make.
  - Hot reloading
    - When you change a source file, the browser automatically reloads.
    - Only tries to reload the relevant files.
  - Enables easy testing and deployment.

## Modules
- CRA uses ES2015 "modules".
- A newer, standardized version of Node's `require()`.
- ***Use this to export/import classes, data, functions between JS files.***
### Notes
#### Exporting
  - `default`
    - When the entire file is required, the default thing that should be exported.
  - Use `{}` when not exporting by default.
#### Importing
  - `./` prefix means look in the folder of the current file.
  - If no `./` prefix, it will look in the modules folder.
  - Can be imported with any name for the "container" for whatever has been exported.
  - *When exported not by default, the names need to match.*
#### To Default or Not?
- Conventionally, default exports are used when there's a "most likely" thing to export out of the file.
  - Ex: `import React from "react";`
- Not necessary to to make a default export but it can be helpful to indicate the most important thing in a file.
### Example
![Modules Example index.js](refImg/modules-example-index-js.png)
![Modules Example helper.js](refImg/modules-example-helper-js.png)

## CRA Conventions for Components
- Each React component goes in separate files.
  - Ex: src/Car.js for Car component.
  - Ex: src/House.js for House component.
  - Ex: src/App.js for App component.
- Components should extend **Component** (imported from React).
  - Export the component as the default object from its given file.
    - Allows for other files to import by doing: `import App from "./App.js";`
  - Example
    - ![Import React Component Example](refImg/import-react-component-example.png)
- Skeletons assumes top-level object is App in App.js.
  - *Best to keep this.*

### CSS and Assets in CRA
- To include images and CSS, we can import them in JS files.
  - Example
    - ![Import CSS and Assets Example](refImg/import-css-and-assets-example.png)
#### CSS
- Make a CSS file for each React component
  - Ex: House.css for House component.
- Import it at the top of the corresponding JS file.
  - Ex: import it at the top of House.js.
  - *CRA will automatically load that CSS.*
- Conventional to add `className="NameOfTheComponent"` onto the top-level element in the component.
  - And use that prefix for sub-items to style.
  - Example
    - ![CSS className Convention Example](refImg/css-classname-convention-example.png)
#### Images
- Store images in src/ folder along with the components.
- Load them where needed, and use imported name where path should go.
  - Example
    - ![Import Images Example](refImg/import-images-example.png)