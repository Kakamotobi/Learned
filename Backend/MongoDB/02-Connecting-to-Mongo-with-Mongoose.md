# Connecting to Mongo with Mongoose

## Table of Contents
- [What is Mongoose?](#what-is-mongoose)
-[Connecting Mongoose to MongoDB](#connecting-mongoose-to-mongodb)
- [CRUD Operations with Mongoos](#crud-operations-with-mongoose)
  - [Inserting with Mongoose - Mongoose Model](#inserting-with-mongoose--mongoose-model)
    - [Models](#models)
    - [Process](#process)
  - [Finding with Mongoose](#finding-with-mongoose)
  - [Updating with Mongoose](#updating-with-mongoose)
  - [Deleting with Mongoose](#deleting-with-mongoose)

## What is Mongoose?
- An ODM that provides ways for us to model out our application data and define a schema. It offers easy ways to validate and build complex queries from the comfort of JS.
  - Object Data/Document Mapper: maps the data that comes back from Mongo and the data that we want to insert into Mongo, into usable JS objects.
- Goal: easily interact with a Mongo database from JS.

## Connecting Mongoose to MongoDB
```
<<terminal>>
npm init -y
touch index.js
npm install mongoose

<<index.js>>
// Require mongoose
const mongoose = require("mongoose");

// Connect to a mongo database and server
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
> Fancy constructors compiled from `Schema` definitions. An instance of a model is called a document. Models are responsible for creating and reading documents from the underlying MongoDB database.
- JS classes that we make with the assistance of Mongoose that represent information of a collection on a Mongo database.
  - i.e., JS class that models the data coming back from and to the Mongo database.
- *Every different resource/collection each have to have a defined model in order to use it in JS.*
#### Process
- Define a Schema.
  - Schema: a mapping of different collection keys from Mongo to different data types in JS.
  - Example
    ```
    const movieSchema = new mongoose.Schema({ title: String, score: Number });
    ```
- Tell Mongoose to make a Model using that Schema.
  - Pass in the name of the Model, and then the pre-defined Schema.
  - Example
    ```
    const Movie = mongoose.model("Movie", movieSchema)
    ```
- Create an instance of a Model and save it to the database.
  - Example
    ```
    const amadeus = new Movie({ title: "Amadeus", score: 9.2 });
    amadeus.save();
    ```
- OR `Model.insertMany([{}, ...])` to insert multiple instances at once.
  - No need to `.save()`.
  - Returns a Promise.
  - Example
    ```
    Movie.insertMany([
      { title: "Amelie", score: 8.3 },
      { title: "Stand by Me", score: 8.6 }
    ])
    .then(data => {
      console.log("It worked!");
      console.log(data);
    });
    ```
- `db.movies.find()` to see the object(s) saved in the collection in the database.
  - Collection is plural of Model (Ex: movies).

### Finding with Mongoose
- **`Model.find()`**
  - Finds documents and returns them in an array.
  - Results to Mongoose queries (promise-like).
    - Queries are "thenable" objects that do not give the data right away.
    - So, **`Model.find().then(data => console.log(data)`**
  - Examples
    ```
    Movie.find({rating: "PG-13"}).then(data => console.log(data));
    Movie.find({year: {$gte: 2010}}).then(data => console.log(data));
    ```
- **`Model.findById()`**
- **`Model.findOne()`**
  - Returns the first match.

### Updating with Mongoose

### Deleting with Mongoose


## Reference
[Mongoose v5.12.12: Getting Started](https://mongoosejs.com/docs/)
