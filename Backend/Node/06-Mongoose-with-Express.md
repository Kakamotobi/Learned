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
    lowercase: true,
    enum: ["fruit", "vegetable", "dairy"]
  },
})

const Product = mongoose.model("Product", productSchema);

module.exports = Product;
```
```js
// seeds.js
// File that we will run on its own anytime we want some new data in our database.

const mongoose = require("mongoose");
const Product = require("./models/product.js");

mongoose
	.connect("mongodb://localhost:27017/farmStand", {
		useNewUrlParser: true,
		useUnifiedTopology: true,
	})
	.then(() => {
		console.log("Mongo Connection Open!");
	})
	.catch((err) => {
		console.log("Mongo Connection Error!");
		console.log(err);
	});

const seedProducts = [
	{
		name: "Fairy Eggplant",
		price: 1.0,
		category: "vegetable",
	},
	{
		name: "Organic Goddess Melon",
		price: 4.99,
		category: "fruit",
	},
	{
		name: "Organic Seedless Watermelon",
		price: 3.99,
		category: "fruit",
	},
	{
		name: "Organic Celery",
		price: 1.5,
		category: "vegetable",
	},
	{
		name: "Chocolate Whole Milk",
		price: 2.69,
		category: "dairy",
	},
];

Product.insertMany(seedProducts)
	.then((res) => {
		console.log(res);
	})
	.catch((err) => {
		console.log(err);
	});
```




