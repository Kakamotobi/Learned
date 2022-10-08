# Working with Type Declarations

## Table of Contents
- [What are Type Declaration Files](#what-are-type-declaration-files)
- [Working with Third-Party Libraries](#working-with-third-party-libraries)
  - [Example - `axios`](#example-axios)
  - [Example - `lodash`](#example-lodash)

## What are Type Declaration Files
- Type Declarations files are special files in TypeScript suffixed with `.d.ts`.
- `.d.ts` files are declaration files that only contain type information.
  - No `.js` outputs are produced.
  - These files inform TypeScript about the types that it needs to know about, and it also serves to us as a guide to working with the API.
- cf. `.ts` files are implementation files that contain types and executable code.

## Working with Third-Party Libraries
- Right click on the imported library and click type definitions to see its type declaration file, which came with its installation.
  - *Not all libraries come with a type declaration file.*
- Most third-party libraries that do not have a type declaration file have a pre-existing type declaration file that has been written for them (needs to be installed separately).
  - `@types/` refers to the `DefinitelyTyped` repository that is a collection of types for popular libraries.
### Example - `axios`
- Axios comes with a type declaration file.
  - Has `"types": "index.d.ts"` in its `package.json`.
```ts
import axios from "axios";

interface User {
  id: number,
  name: string,
  username: string,
  email: string,
  address: {
    street: string,
    suite: string,
    city: string,
    zipcode: string,
    geo: {
      lat: string,
      lng: string
    }
  },
  phone: string,
  website: string,
  company: {
    name: string,
    catchPhrase: string,
    bs: string
  }
}

axios
  .get<User>("https://jsonplaceholder.typicode.com/users/1")
  .then(res => {
    printUser(res.data); // `res.data` is of type `User`
  })
  .catch(err => {
    console.log(err);
  });

axios
  .get<User[]>("https://jsonplaceholder.typicode.com/users")
  .then(res => {
    res.data.forEach(printUser); // `res.data` is of type `User[]`
  })
  .catch(err => {
    console.log(err);
  });

function printUser(user: User) {
  console.log(user.name);
  console.log(user.email);
}
```
### Example - `lodash`
- Lodash does not come with a type declaration file.
```zsh
npm install lodash;
npm install --save-dev @types/lodash;
```
```ts
import _ from "lodash";

_.sample([4,2,563,14,3,22,6]);
```
