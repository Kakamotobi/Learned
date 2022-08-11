# Working with TypeScript in a Project

## Table of Contents
- [Setup](#setup)
- [Working with the DOM](#working-with-the-dom)
  - [DOM Types](#dom-types)
    - [Approach 1 - JS Optional Chaining (`?`)](#approach-1-js-optional-chaining-?)
    - [Approach 2 - TS Non-Null Assertion Operator (`!`)](#approach-2-ts-non-null-assertion-operator-!)
  - [Type Assertions](#type-assertions)
    - [Type Assertions and the DOM](#type-assertions-and-the-dom)
- [Example Project](#example-project)

## Setup
- **Ordinary project setup.**
  - Ex: `npm init`, `git init`, etc.
- **Initialize TypeScript**
  - `tsc --init`
- **Setup TypeScript configurations.**
  - `outDir`, `include`, etc.
    - `src`, `dist` folders.
  - Link up output file to HTML.
- **Compile in watch mode.**
  - `tsc -w`
- **Start live server.**
  - Ex: `lite-server` 

## Working with the DOM
### DOM Types
- TypeScript knows that the DOM node is an HTML element, but it doesn't know what element it is (ex: button).
- Hence it is typed to a generic `HTMLElement` type. 
- Since it may or may not exist, it is union typed with `null`.
- Example
  ```ts
  const btn = document.querySelector("btn"); // const btn: HTMLElement | null
  ```
- Since it may be `null`, TS does not allow us to use methods and properties right off the bat.
- Therefore, we have to account for that through the use of **Optional Chaining** or the **Non-Null Assertion Operator**.
#### Approach 1 - JS Optional Chaining (`?`)
```ts
btn?.addEventListener("click", () => alert("Clicked!"));
```
#### Approach 2 - TS Non-Null Assertion Operator (`!`)
- The non-null assertion operator tells TS that this variable is guaranteed not to be `null`.
- Use this for cases where it is for certain.
```ts
const btn = document.getElementById("btn")!;
    
btn.addEventListener("click", () => alert("Clicked!"));
```
### Type Assertions
- You may have more information about the type of a value that TypeScript cannot know about.
- Type asserting is a way to specify a more specific type.
#### Example
- The `mystery` variable is typed to `unknown`.
- However, by using type assertion, it can be treated as a `string`.
```ts
let mystery: unknown = "Hello World!!!";

const numChars = mystery.length; // Error
const numChars = (mystery as string).length;
```
- Since this is pre-runtime, the following works:
  ```ts
  let mystery: unknown = 4;
  
  const numChars = (mystery as string).length;
  ```
  - However, it will lead to an error when the code runs.
#### Type Assertions and the DOM
- The generic `HTMLElement` does not have specific methods and properties of different elements.
  ```ts
  const input = document.querySelector("input")!;

  input.value; // Property 'value' does not exist on type 'HTMLElement'.
  ```
- Therefore, you need to specify the type using type assertion.
  ```ts
  const input = document.querySelector("input")! as HTMLInputElement;
  
  input.value;
  ```
  ```ts
  // Alternative Syntax for Type Assertion
    // Does not work with JSX.
  
  const input = document.querySelector("input")!;
  
  (<HTMLInputElement>input).value;
  ```

## Example Project
```html
<body>
  <ul></ul>
  <form>
    <input type="text" />
    <button type="submit">Submit</button>
  </form>
</body>
```
```js
interface Todo {
  text: string;
  completed: boolean;
}

const form = document.querySelector("form")!;
const input = form.querySelector("input")! as HTMLInputElement;
const list = document.querySelector("ul")!;

const todos: Todo[] = readTodosFromLS();
todos.forEach(createTodoElement);

function readTodosFromLS(): Todo[] {
  const todosJSON = localStorage.getItem("todos");
  if (todosJSON === null) return [];
  return JSON.parse(todosJSON);
}

function saveTodosToLS() {
  localStorage.setItem("todos", JSON.stringify(todos));
}

function createTodoElement(todo: Todo) {  
  const newLi = document.createElement("li");
  const checkbox = document.createElement("input");
  checkbox.type = "checkbox";
  checkbox.checked = todo.completed;
  checkbox.addEventListener("change", () => {
    todo.completed = checkbox.checked;
    saveTodosToLS();
  });
  
  newLi.append(todo.text);
  newLi.append(checkbox);
  list.append(newLi);
}

function handleSubmit(evt: SubmitEvent) {
  evt.preventDefault();
  
  const newTodo: Todo = {
    text: input.value,
    completed: false,
  }
  
  // State
  todos.push(newTodo);
  
  // LocalStorage
  saveTodosToLS();
  
  // UI
  createTodoElement(newTodo);
  
  input.value = "";
}

form.addEventListener("submit", handleSubmit);
```
