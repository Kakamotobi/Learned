# Connecting to Mongo with Mongoose

## Table of Contents
- [What is Mongoose?](#what-is-mongoose)
- [Connecting Mongoose to MongoDB](#connecting-mongoose-to-mongodb)
- [CRUD Operations with Mongoose](#crud-operations-with-mongoose)
  - [Inserting with Mongoose - Mongoose Model](#inserting-with-mongoose---mongoose-model)
    - [Models](#models)
    - [Process](#process)
  - [Mongoose Queries](#mongoose-queries)
  - [Finding with Mongoose](#finding-with-mongoose)
  - [Updating with Mongoose](#updating-with-mongoose)
  - [Deleting with Mongoose](#deleting-with-mongoose)
- [Mongoose Schema Validations](#mongoose-schema-validations)
- [Adding Model Instance Methods](#adding-model-instance-methods)
- [Adding Model Static Methods](#adding-model-static-methods)
- [Mongoose Virtuals](#mongoose-virtuals)
- [Defining Mongoose Middleware](#defining-mongoose-middleware)

## What is Mongoose?
- An ODM that provides ways for us to model out our application data and define a schema. It offers easy ways to validate and build complex queries from the comfort of JS.
  - Object Data/Document Mapper: maps the data that comes back from Mongo and the data that we want to insert into Mongo, into usable JS objects that we can add methods, validations/conditions to.
- Goal: easily interact with a Mongo database from JS (i.e., control what's happening in Mongo, in JS through Mongoose).
- Operation Buffering
  - Mongoose lets you start using your models immediately without waiting for mongoose to establish a connection to MongoDB.
- A way to connect a Node app with MongoDB.
  - Ex: when a user creates an account, we need to store that in MongoDB.

## Connecting Mongoose to MongoDB
```zsh
//terminal

npm init -y
npm install mongoose
touch index.js
```
```js
//index.js

// Require mongoose
const mongoose = require("mongoose");

// Connect to a mongo database and server
// In the URI, we are specifying where MongoDB is being served locally and which database to use.
mongoose
    // DB Name: movieApp
    .connect("mongodb://localhost:27017/movieApp", {
        useNewUrlParser: true,
        useUnifiedTopology: true,
    })
    .then(() => {
        console.log("Connection Open!");
    })
    .catch((err) => {
        console.log("Connection Error!");
        console.log(err);
    });
```

## CRUD Operations with Mongoose
### Inserting with Mongoose - Mongoose Model
#### Models
> Fancy constructors compiled from `Schema` definitions. An instance of a model is called a document. ***Models are responsible for creating and reading documents from the underlying MongoDB database.***
- JS classes that we make with the assistance of Mongoose that represent information of a collection on a Mongo database.
  - i.e., JS class that models the data coming back from and to the Mongo database.
- *Every different resource/collection each have to have a defined model in order to use it in JS.*
#### Process
- **Define a Schema.**
  - Schema: a mapping of different collection keys from Mongo to different data types in JS.
  - Example
    ```js
    const movieSchema = new mongoose.Schema({
      title: String, 
      score: Number
    });
    ```
- **Tell Mongoose to make a Model using that Schema.**
  - Pass in the name of the Model, and then the pre-defined Schema.
    - *The model name has to be capitalized and singular.*
    - *Mongoose will then take the name and create a collection (non-capital, pluralized).*
  - Example
    ```js
    const Movie = mongoose.model("Movie", movieSchema); // Model class
    ```
- **Create an instance of the Model class and save it to the database.**
  - *`.save()` returns a promise.*
  - Example
    ```js
    const amadeus = new Movie({ title: "Amadeus", score: 9.2 });
    amadeus.save();
    ```
- **OR `<Model>.insertMany([{}, ...])` to insert multiple instances at once.**
  - No need to `.save()`.
  - Returns a Promise.
  - Example
    ```js
    Movie.insertMany([
      { title: "Amelie", score: 8.3 },
      { title: "Stand by Me", score: 8.6 }
    ])
    .then(data => {
      console.log("It worked!");
      console.log(data);
    });
    ```
- **`db.movies.find()` to see the object(s) saved in the collection in the database.**
  - Collection is plural of Model (Ex: movies).

### Mongoose Queries
- *Promise-like* objects.
  - Mongoose Queries are "thenable' objects that do not give the data right away.
  - So, attach **`.then(data => console.log(data)`**.

- Mongoose functions that return a mongoose `Query` object [here](https://mongoosejs.com/docs/queries.html).

### Finding with Mongoose
- **`<Model>.find()`**
  - Finds documents and returns them in an array.
  - Examples
    ```js
    Movie.find({rating: "PG-13"}).then(data => console.log(data));
    Movie.find({year: {$gte: 2010}}).then(data => console.log(data));
    ```
- **`<Model>.findById()`**
- **`<Model>.findOne()`**
  - Returns the first match.

### Updating with Mongoose
#### Methods that don't resolve with the updated information (only the number of modifications that was made)
- **`<Model>.updateOne()`**
  - Update the first document that matches the filter.
  - Ex: `Movie.updateOne({ title: "Amadeus"}, { year: 1984 }).then(res => console.log(res));`
- **`<Model>.updateMany()`**
  - Update all documents that match the filter.
  - Ex: `Movie.updateMany({title: {$in: ["Amadeus", "Stand By Me"]}}, {score: 10}).then(res => console.log(res));`
- **`<Model>.update()`**
  - Updates one document in the database without returning it.
#### Methods that resolve with the updated document
- **`<Model>.findOneAndUpdate()`**
  - Finds a matching document, updates it according to the update arg, passing any options, and returns the found document (if any) to the callback.
  - By default, returns the old version.
  - To return the updated version, specify option: **`{new: true}`**.
    - Ex:`Movie.findOneAndUpdate({title: "The Iron Giant}, {score: 7.8}, {new: true}).then(res => console.log(res));`
- **`<Model>.findByIdAndUpdate()`**
  - Finds a single document by its `_id` field.

### Deleting with Mongoose
- **`<Model>.remove()`**
  - Removes all documents that match conditions from the collection.
  - Doesn't return the documents that were involved. Just returns the deleted count.
- **`<Model>.findOneAndRemove()`**
- **`<Model>.findByIdAndRemove()`**

## Mongoose Schema Validations
### Shorthand Approach
```js
const productSchema = new mongoose.Schema({
  name: String,
  price: Number
});
```
### Another Approach
```js
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  price: {
    type: Number,
    required: true,
  },
});
```
- Additional validations can be added.
### Validation Scenarios
- If `required` information is not given, error.
- If specified type in Schema and the type of data given is a mismatch, error.
- If the provided information was unspecified in the Schema, it will simply be ignored.
### Additional Schema Constraints - SchemaType Options
#### Example
```js
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    maxlength: 20,
  },
  price: {
    type: Number,
    required: true,
    min: 0,
  },
  onSale: {
    type: Boolean,
    default: false,
  },
  categories: [String],
  qty: {
    online: {
      type: Number,
      default: 0,
    },
    inStore: {
      type: Number,
      default: 0,
      },
  },
});

const Product = mongoose.model("Product", productSchema);

const bike = new Product({
  name: "Bike Helment",
  price: 29.95,
  categories: ["Cycling", "Safety"],
});

bike.save()
  .then((data) => {
    console.log("It worked!");
    console.log(data);
  })
  .catch((err) => {
    console.log("Oh no error!");
    console.log(err);
  });
```
### Validating Mongoose Updates
- By default, Schema validations are applied when creating new documents, but not when updating them.
- **`{runValidators: true}`**
  - Validate the update operation against the model's schema.
  - Example
    ```js
    Product.findOneAndUpdate({ name: "Tire Pump" }, { price: -19.99 }, { new: true, runValidators: true })
      .then(data => {
        console.log("It worked!");
        console.log(data);
      })
      .catch(err => {
        console.log("Oh no error!");
        console.log(err);
       });
    ```
### Mongoose Validation Errors
- We can set up custom validator error messages.
  - The second value of an array will be the error message.
  - Ex: `min: [0, "Price must be positive!]`
- **`enum`**
  - Creates a validator that checks if the value is in the given array.
  - Ex: `size: {type: String, enum: ["S", "M", "L"]}`

## Adding Model Instance Methods
- Instances of `Models` are documents. Documents have many of their own built-in instance methods. We may also define our own custom document instance methods.
- `this` keyword refers to the individual instance.
  - 
### Examples
```js
productSchema.methods.greet = function () {
  console.log("Hello!!!!");
  console.log(`- from ${this.name}`);
};
productSchema.methods.toggleOnSale = function () {
  this.onSale = !this.onSale;
  return this.save();
};
productSchema.methods.addCategory = function (newCategory) {
  this.categories.push(newCategory);
  return this.save();
};

const Product = mongoose.model("Product", productSchema);

const findProduct = async () => {
  const foundProduct = await Product.findOne({ name: "Bike Helmet" });
  console.log(foundProduct);
  await foundProduct.toggleOnSale();
  console.log(foundProduct);
  await foundProduct.addCategory("Outdoors");
  console.log(foundProduct);
};

findProduct();
```

## Adding Model Static Methods
- Methods that live on the `Model` itself, not on instances.
- `this` keyword refers to the `Model` class itself, not the instance.
- *Usually create model static methods that are customized versions of finding, updating, removing methods to the `Model` itself (doesn't have to do with instances).*
### Example
```js
productSchema.statics.fireSale = function () {
  return this.updateMany({}, { onSale: true, price: 0 });
};

const Product = mongoose.model("Product", productSchema);

Product.fireSale().then((res) => console.log(res));
```

## Mongoose Virtuals
> Virtuals are document properties that you can `get` and `set` but that do not get persisted to MongoDB. The getters are useful for formatting or combining fields, while setters are useful for de-composing a single value into multiple values for storage.
  - Doesn’t exist in the database. Only on the mongoose side of things in JS.
- Usually use these if we have some information that we are accessing quite commonly that we could derive from existing data.
- Ex: we don't need to store a fullname on the database but it would be nice to have a property that we could access as if fullname was in the database.
  - We could store the fullname in the database but why bother if we already have that data (no reason to take up more space.
### Example
```js
// person.js
const personSchema = new mongoose.Schema({
  first: String,
  last: String,
});
personSchema.virtual(“fullName”).get(function () {
  return `${this.first} ${this.last}`;
});

const Person = mongoose.model(“Person”, personSchema);
```
```zsh
// Node REPL

const tom = new Person({first: "Tom", last: "Jerry"});
tom; // { _id: xxxxx, first: "Tom", last: "Jerry"}
tom.fullName; // "Tom Jerry"
```

## Defining Mongoose Middleware
> Middleware (also called pre and post hooks) are functions which are passed control during execution of asynchronous functions. Middleware is specified on the schema level and is useful for writing plugins.
- **`.pre`**
 - `.pre` middleware will execute before a particular method.
- **`.post`**
  - `.post` middleware will execute after a particular method.
### Example
```js
// Runs before save executes.
personSchema.pre(“save”, async function () {
  this.first = “Troll”;
  this.last = “Lolol”;
  console.log(“About to save!!!”);
});

// Runs after save executes.
personSchema.post(“save”, async function () {
  console.log(“Just saved!!”);
});

const Person = mongoose.model(“Person”, personSchema);
```

## Reference
[Mongoose v5.12.12: Getting Started](https://mongoosejs.com/docs/)
