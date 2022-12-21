# Tools for Code

## Prettier
- **A code formatter that enforces consistent code format/style.**
  - Works for JS, TS, JSX, JSON, CSS, SCSS, HTML, etc.
- Configurations (Ex: single/double quotes, line width) can be set per project basis or VS Code user settings, which is the default if the project does not have a specific configuration.
- Configurations can be set through VS code settings or in a config file (Ex: `.prettierrc.json`) in the root directory.

## ESLint
- **A tool for identifying and reporting problems with your JS code, effectively allowing developers to follow a common code style.**
- ESLint can be installed and configured with build tools like Webpack and Vite.
- Configurations (Ex: frontend framework plugin, prettier plugin, `ecmaVersion`) can be set per project or VS Code user settings. 
- Configurations can be set through VS code settings or in a config file (Ex: `.eslintrc.json`) in the root directory.
- It is common to use existing ESLint configurations (Ex: Airbnb's config).

## Setting Up Prettier and ESLint Together
- Install the Prettier and ESLint extensions in VS Code.
- Install dependencies (`npm i -D eslint-config-prettier eslint-plugin-prettier
`).
  - `eslint-config-prettier` ignores all ESLint rules that may conflict with Prettier.
  - `eslint-plugin-prettier` integrates Prettier rules into ESLint rules.
- Setup the config file (`.eslintrc.json`) with the dependencies.
  ```json
  {
    "extends": ["prettier"],
    "plugins": ["prettier"],
    "rules": {
      "prettier/prettier": ["error"]
    }
    // ...
  }
  ```
- The `.prettierrc.json` file contains your Prettier settings.
  ```json
  {
    "singleQuote": false,
    "printWidth": 120
  }
  ```

## Reference
[Configuration File · Prettier](https://prettier.io/docs/en/configuration.html)  
[How to use Prettier with ESLint](https://www.robinwieruch.de/prettier-eslint/)  
