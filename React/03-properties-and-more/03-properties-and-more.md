# Properties and More

## Properties
- A useful component is a reusable one, which often means making it configurable or customizable.
- Way of passing data from one component to another (from a parent to a child).
### Using Properties
- Go to wherever you're calling upon a component and pass data inside the component tag (just like passing in a `src` attribute for an image).
- `console.log(this.props)` to see props object.
#### Example
![Props Example index.js](refImg/props-example-index-js)
![Props Example Hello.js](refImg/props-example-hello-js)
### Properties Requirements
- **Properties are for *configuring* your component.**
- **Properties are *immutable*.**
  - Ex: `this.props.from = "Topsy"` results in an error.
  - *Doesn't mean that the data in your application cannot change or that your components can never be altered.*
  - *But we don't do it through props.*

## Other Types of Properties
- Properties can be strings.
  - Ex: `<User name="Tom" title="Cat"/>`
- For other types, embed JS expression using `{}`.
  - Ex: `<User name="Tom" salary={999999} hobbies={["chasing", "piano", "games"]}/>`
### Example
![Types of Props Example index.js](refImg/types-of-props-example-index-js)
![Types of Props Example Hello.js](refImg/types-of-props-example-hello-js)

