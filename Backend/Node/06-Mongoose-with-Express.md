# Mongoose with Express

## Table of Contents
- [Express + Mongoose Basic Setup](#express-mongosee-basic-setup)

## Express + Mongoose Basic Setup
```zsh
// Terminal

npm init -y
npm i express ejs mongoose
touch index.js
mkdir views
```
```js
// index.js

const express = require("express");
const app = express();
const path = require("path");
const mongoose = require("mongoose");

const Product = require("./models/product.js");

mongoose.connect('mongodb://localhost:27017/farmStand', {useNewUrlParser: true, useUnifiedTopology: true})
  .then(() => {
    console.log("Mongo Connection Open!")
  })
  .catch(err => {
    console.log("Mongo Connection Error!");
    console.log(err);
  })

app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

app.get("/dog", (req, res) => {
  res.send("woof")
})

app.listen(3000, () => {
  console.log("Listening on port 3000!")
})
```
```js
//product.js
const mongoose = require("mongoose");

const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    required: true,
    min: 0
  },
  category: {
    type: String,
    enum: ["fruit", "vegetable", "dairy"]
  },
})

const Product = mongoose.model("Product", productSchema);

module.exports = Product;

```





