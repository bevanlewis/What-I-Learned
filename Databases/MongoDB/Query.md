# Query

## Find

`db.collection.find()` - Find document(s) in a collection where query = item. Returns 1 or more. Returns a cursor

```shell
db.collection.find({query: "item"})
```

`db.collection.findOne()` - Find exactly one item. Returns a document.

```shell
db.collection.findOne({query: "item"})
```

To find an element when searching for an object nested in an object

```shell
db.collection.findOne({'obj1Key.obj2Key`: "obj2Item"})
```

## Comparision operators

| Operator | Description              | Example                                          |
| -------- | ------------------------ | ------------------------------------------------ |
| `$gt`    | Greater than             | `db.inventory.find({qty: {$gt: 20}})`            |
| `$lt`    | Less than                | `db.inventory.find({qty: {$lt: 20}})`            |
| `$lte`   | Less than or equal to    | `db.inventory.find({qty: {$lte: 20}})`           |
| `$gt`    | Greater than             | `db.inventory.find({qty: {$gt: 20}})`            |
| `$gte`   | Greater than or equal to | `db.inventory.find({qty: {$gte: 20}})`           |
| `$eq`    | Equal to                 | `db.inventory.find({qty: {$eq: 20}})`            |
| `$ne`    | Not equal to             | `db.inventory.find({qty: {$ne: 20}})`            |
| `$in`    | In an array              | `db.inventory.find({qty: {$in: [20, 30, 40]}})`  |
| `$nin`   | Not in an array          | `db.inventory.find({qty: {$nin: [20, 30, 40]}})` |
