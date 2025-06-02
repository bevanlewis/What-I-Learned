# Mongoose Basics

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a higher-level, schema-based solution to model your application data, making it easier to work with MongoDB in JavaScript.

## Connecting to MongoDB

First, require Mongoose and connect to your MongoDB database:

```javascript
const mongoose = require("mongoose");

mongoose.connect("mongodb://127.0.0.1:27017/test", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
```

## Defining a Schema and Model

A schema defines the structure of your documents. A model is a wrapper for the schema and provides methods to interact with the database.

```javascript
const catSchema = new mongoose.Schema({
  name: String,
  age: Number,
});

const Cat = mongoose.model("Cat", catSchema);
```

## Insert Documents

Add a new document to the collection:

```javascript
const kitty = new Cat({ name: "Zildjian", age: 3 });
kitty
  .save()
  .then(() => console.log("Cat saved!"))
  .catch((err) => console.error(err));
```

Or insert many:

```javascript
Cat.insertMany([
  { name: "Milo", age: 2 },
  { name: "Otis", age: 4 },
])
  .then(() => console.log("Cats added!"))
  .catch((err) => console.error(err));
```

## Query Documents

Find all documents:

```javascript
Cat.find({})
  .then((cats) => console.log(cats))
  .catch((err) => console.error(err));
```

Find one document:

```javascript
Cat.findOne({ name: "Milo" })
  .then((cat) => console.log(cat))
  .catch((err) => console.error(err));
```

Find by ID:

```javascript
Cat.findById("someObjectId")
  .then((cat) => console.log(cat))
  .catch((err) => console.error(err));
```

## Update Documents

Update one document:

```javascript
Cat.updateOne({ name: "Milo" }, { $set: { age: 3 } })
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

Update many documents:

```javascript
Cat.updateMany({}, { $set: { age: 5 } })
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

## Delete Documents

Delete one document:

```javascript
Cat.deleteOne({ name: "Otis" })
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

Delete many documents:

```javascript
Cat.deleteMany({ age: { $gt: 4 } })
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

---

For more details and advanced usage, see the [Mongoose documentation](https://mongoosejs.com/).
