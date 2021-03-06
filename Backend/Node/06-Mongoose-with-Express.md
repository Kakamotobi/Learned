# Mongoose with Express

## Table of Contents
- [Express + Mongoose Basic Setup (CRUD)](#express--mongoose-basic-setup-crud)

## Express + Mongoose Basic Setup (CRUD)
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
const methodOverride = require("method-override");

const Product = require("./models/product.js");

// -----Mongoose connect----- //
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

// -----EJS----- //
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

// -----Middleware----- //
app.use(express.urlencoded({ extended: true }));
app.use(methodOverride("_method"));

// -----Categories----- //
const categories = ["fruit", "vegetable", "dairy"];

// -----Routes----- //
// Serve the form
app.get("/products/new", (req, res) => {
	res.render("products/new.ejs", { categories });
});
// Route to submit the form
app.post("/products", async (req, res) => {
	const newProduct = new Product(req.body);
	await newProduct.save();
	res.redirect(`/products/${newProduct._id}`);
});

// All products
app.get("/products", async (req, res) => {
	const { category } = req.query;
	if (category) {
		const products = await Product.find({ category });
		res.render("products/index.ejs", { products, category });
	} else {
		const products = await Product.find({});
		res.render("products/index.ejs", { products, category: "All" });
	}
});

// Product details
app.get("/products/:id", async (req, res) => {
	const { id } = req.params;
	const product = await Product.findById(id);
	res.render("products/show.ejs", { product });
});

// Serve form to edit product
app.get("/products/:id/edit", async (req, res) => {
	const { id } = req.params;
	const product = await Product.findById(id);
	res.render("products/edit.ejs", { product, categories });
});
// Route to submit the form
app.put("/products/:id", async (req, res) => {
	const { id } = req.params;
	const product = await Product.findByIdAndUpdate(id, req.body, {
		runValidators: true,
		new: true,
	});
	res.redirect(`/products/${product._id}`);
});

// Delete product
app.delete("/products/:id", async (req, res) => {
	const { id } = req.params;
	const deletedProduct = await Product.findByIdAndDelete(id);
	res.redirect("/products");
});

// -----Port----- //
app.listen(3000, () => {
	console.log("Listening on port 3000!");
});
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
```ejs
// index.ejs

<body>
	<h1><%= category %> Products</h1>
	<ul>
		<% for (let product of products) { %>
		<li><a href="/products/<%= product._id %>"><%= product.name %></a></li>
		<% } %>
	</ul>

	<a href="/products/new">Add New Product</a>
	<% if (category !== "All") {%>
	<a href="/products">All Products</a>
	<% } %>
</body>
```
```ejs
// show.ejs

<body>
	<h1><%= product.name %></h1>
	<ul>
		<li>Price: $<%= product.price %></li>
		<li>
			Category:
			<a href="/products?category=<%= product.category %>"
				><%= product.category %></a
			>
		</li>
	</ul>

	<a href="/products">All Products</a>
	<a href="/products/<%= product._id %>/edit">Edit Product</a>
	<form action="/products/<%= product._id %>?_method=DELETE" method="POST">
		<button>Delete</button>
	</form>
</body>
```
```ejs
// new.ejs

<body>
	<h1>Add A Product</h1>

	<form action="/products" method="POST">
		<label for="name">Product Name</label>
		<input type="text" name="name" id="name" placeholder="Product Name" />
		<label for="price">Price (Unit)</label>
		<input
			type="number"
			name="price"
			id="price"
			placeholder="Product Price"
		/>
		<label for="category">Select Category</label>
		<select name="category" id="category">
			<% for (let category of categories) { %>
			<option value="<%=category%>"><%=category%></option>
			<% } %>
		</select>
		<button type="submit">Add Product</button>
	</form>
</body>
```
```ejs
// edit.ejs

<body>
	<h1>Edit Product</h1>

	<form action="/products/<%= product._id %>?_method=PUT" method="POST">
		<label for="name">Product Name</label>
		<input
			type="text"
			name="name"
			id="name"
			placeholder="Product Name"
			value="<%= product.name %>"
		/>
		<label for="price">Price (Unit)</label>
		<input
			type="number"
			name="price"
			id="price"
			placeholder="Product Price"
			value="<%= product.price %>"
		/>
		<label for="category">Select Category</label>
		<select name="category" id="category">
			<% for (let category of categories) { %>
			<option value="<%=category%>" <%= product.category === category ? "selected" : "" %> ><%=category%></option>
			<% } %>
		</select>
		<button type="submit">Add Product</button>
	</form>

	<a href="/products/<%= product._id %>">Cancel</a>
</body>
```


