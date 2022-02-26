# Introduction to MongoDB

## Table of Contents
- [What is it?](#what-is-it)
  - [Document-Oriented Database / Document Data Store](#document-oriented-database--document-data-store)
- [Types of Databases](#types-of-databases)
  - [SQL Databases](#sql-databases)
  - [NoSQL Databases](#nosql-databases)
- [Installing MongoDB (macOS community edition)](#installing-mongodb-macos-community-edition)
- [The Mongo Shell](#the-mongo-shell)
- [Binary JSON (BSON)](#binary-json-bson)
- [MongoDB CRUD Operations](#mongodb-crud-operations)
  - [Inserting with Mongo](#inserting-with-mongo)
  - [Finding/Querying with Mongo](#findingquerying-with-mongo)
  - [Updating with Mongo](#updating-with-mongo)
  - [Deleting with Mongo](#deleting-with-mongo)
  - [Atomic Operators](#atomic-operators)
    - [Query and Projection Operators](#query-and-projection-operators)
    - [Update Operators](#update-operators)
    - [Logical Operators](#logical-operators)

## What is it?
- A document database, which we can use to store and retrieve complex data from.
- Very commonly used with Node and Express (MEAN & MERN stacks).
- Plays particularly well with JavaScript.
### Document-Oriented Database / Document Data Store
- A type/category of NoSQL database.
- Store information in some format (ex: JSON) to a document.
  - Instead of rows and columns of data, data is stored in JSON format.
  - MongoDB allows us to create, edit, delete, search these documents.
- Take data as it is and store all of its relevant information in a single instance in the database, without having to store them in a table that follows a pre-defined schema.

## Types of Databases
### SQL Databases
- Structured Query Language databases are relational databases.
- We pre-define a schema of tables before we insert anything.
- Everything is done in table and has to conform to a pattern.
- Ex: MySQL, Postgres, SQLite, Oracle, Microsoft SQL Server.
### NoSQL Databases
- NoSQL databases do not use SQL. 
- There are many types of no-sql databases, including document, key-value, and graph stores.
- Ex: MongoDB, Couch DB, Neo4j, Cassandra, Redis.

## Installing MongoDB (macOS community edition)
1. Install Homebrew
  - `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Tap the MongoDB Homebrew Tap
  - `brew tap mongodb/brew`
3. Install MongoDB
  - `brew install mongodb-community`
4. Run MongoDB as a macOS service
  - `brew services start mongodb-community`
  - Stopping running a mongod running as a macOS service
    - `brew services stop mongodb-community`
5. Verify that MongoDB is running as a macOS service.
  - `brew services list`
6. Begin using MongoDB
  - `mongo`
  - If error: "zsh: command not found: mongo"
    1. `cd ~`.
    2. `ls -a` to check if `.zshrc` file exists.
    3. If it doesn't, `touch .zshrc`.
    4. Append the path to the file by doing: `echo export PATH="$PATH:/usr/local/Cellar/mongodb-community@4.4/4.4.5/bin" >> .zshrc`.
    5. Verify that it has been properly added by doing: `cat .zshrc`.
    6. `mongo` works now.
### Reference
[Install MongoDB Community Edition on macOS - MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)  
[remove mongodb that was installed via brew](https://gist.github.com/katychuang/10439243)  
[mongodb/homebrew-brew: The Official MongoDB Software Homebrew Tap](https://github.com/mongodb/homebrew-brew)  

## The Mongo Shell
- **`mongo`**
  - Initiate the Mongo Shell (REPL).
  - *Mongod needs to be running in the background as it will try to connect to that daemon.*
- **`help`**
  - Shows some commands that we can use.
- **`db`**
  - The current database.
  - Returns "test" if not using any.
- **`show dbs`**
  - Prints a list of all databases on the server.
- **`use <db>`**
  - Switch current database to that db.
  - If that db doesn't exist, it will be created and switched to.  
    - Doesn't show in `show dbs` until there's something in it.
- **`show collections`**
  - Shows the collections (groupings of data in the current db).
- **ctrl + c**
  - Quit mongo.

## Binary JSON (BSON)
> A binary representation to store data in JSON format, optimized for speed, space, and flexibility (data support).
- Binary JSON that is parsed quicker than JSON, which is a text-based format.
### Reference
[JSON and BSON | MongoDB](https://www.mongodb.com/json-and-bson)

## MongoDB CRUD Operations
### Inserting with Mongo
- When inserting, we need to insert into a collection.
  - Collection: grouping of data in a database.
  - Inserting into a collection that doesn't exist will make that collection.
- Ways to Insert
  - **`db.<collection>.insertOne({})`**
    - Insert a single document into a collection.
    - Ex: `db.dogs.insert({name: "Charlie", age: 3, breed: "corgi", catFriendly: true})`
  - **`db.<collection>.insertMany([{},{}])`**
    - Insert multiple documents into a collection.
    - Expects an array of documents to be passed in.
  - **`db.<collection>.insert([{},{}])`**
    - Insert a single or multiple documents into a collection.
    - Ex: `db.dogs.insert([{name: "Wyatt", breed: "Golden Retriever", age: 14, catFriendly: false}, {name: "Tonya", breed: "Chihuahua", age: 17, catFriendly: true}])`
    - If an inserted document omits the `_id` field, the MongoDB driver automatically generates an ObjectId for the `_id` field.
      - *A unique reference for each individual document in a collection.*
### Finding/Querying with Mongo
- **`db.<collection>.find({})`**
  - Selects documents in a collection or view and return a cursor to the selected documents.
    - Cursor: a pointer/reference to the results of a query.
  - Ex: `db.dogs.find()`
  - Ex: `db.dogs.find({breed: "corgi"})`
  - Ex: `db.dogs.find({catFriendly: true, age: 17})`
- **`db.<collection>.findOne({})`**
  - Finds one document and return it.
### Updating with Mongo
- **`db.<collection>.updateOne(<filter>, <update>, <options>)`**
  - Updates the first thing that matches.
  - Filter: find a document based upon some criteria.
  - Update: use atomic operator.
  - Ex: `db.dogs.updateOne({name: "Charlie"}, {$set: {age: 4, breed: "Lab"}})`
    - Find Charlie and set its age to 4, and breed to "Lab".
  - If setting something that is not currently in the matched document, it will create and include the key-value pair in the document.
    - Ex: `db.dogs.updateOne({name: "Charlie"}, {$set: {color: "chocolate"}})`
- **`db.<collection>.updateMany(<filter>, <update>, <options>)`**
  - Update many documents at once.
  - Ex: `db.dogs.updateMany({catFriendly: true}, {$set: {isAvailable: false}})`
- **`db.<collection>.replaceOne(<filter>, <update>, <options>)`**
  - Replaces a single document within the collection based on the filter.
### Deleting with Mongo
- **`db.<collection>.deleteOne()`**
  - Removes a single document from a collection.
  - Ex: `db.cats.deleteOne({name: "Blue Steele"})`
  - *Empty `{}` will delete the first document returned in the collection.*
- **`db.<collection>.deleteMany()`**
  - Removes multiple documents from a collection.
  - Ex: `db.dogs.deleteMany({isAdopted: true})`
  - *Empty `{}` will delete everything in the collection.
### Atomic Operators
#### Query and Projection Operators
- **`$gt`**
  - Mathces values that are greater than a specified value.
  - Ex: `db.dogs.find({age: {$gt: 8}})`
- **`$gte`**
  - Matches values that are greater than or equal to a specified value.
- **`$lt`**
  - Matches values that are less than a specified value.
- **`$lte`**
  - Matches values that are less than or equal to a specified value.
- **`$in`**
  - Selects the documents where the value of a field equals any value in the specified array.
  - Ex: `db.dogs.find({breed: {$in: ["Mutt", "Corgi"]}})`
- **`$nin`**
  - Selects the documents where the value of the field is not in the specified array OR the field does not exist.
  - Ex: `db.dogs.find({breed: {$nin: ["Mutt", "Corgi"]}})`
- **`$ne`**
  - Selects the documents where the value of the field is not equal to the specified value.
##### Tip
- *Accessing sub documents.*
  - Ex: `db.dogs.find({"personality.childFriendly": true, size: "M"})`
#### Update Operators
- **`{$set: {<field1>: <value1>, ...}}`**
  - Replaces the value of a field with the specified value.
- **`{$currentDate: {<field1>: <typeSpecification1>, ...}}`**
  - Sets the value of a field to the current date, either as a Date or a Timestamp.
  - Ex: `{$currentDate: {lastModified: true}}`
#### Logical Operators
- Use these to combine multiple operators.
- **`$and`**
- **`$not`**
- **`$nor`**
- **`$or`**
  - Ex: `db.dogs.find({$or: [{"personality.catFriendly": true}, {age: {$lte: 20}}]})`

### Reference
[MongoDB CRUD Operations - MongoDB Manual](https://docs.mongodb.com/manual/crud/)
[mongo Shell Methods - MongoDB manual](#https://docs.mongodb.com/manual/reference/method/)
[Operators - MongoDB Manual](https://docs.mongodb.com/manual/reference/operator/)

