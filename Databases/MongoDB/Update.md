# Update

## Update documents

`db.collection.updateOne()` - to update one item.

First part does a find.

If an item does not exist already then it is added as a new key value pair.

```shell
db.collection.updateOne({query: "item"}, {$set: {item1: 4, item2: 6}, $otherModifer...})
```

`db.collection.updateMany()` - to update mnay items.

```shell
db.collection.updateMany({}, {$set: {item1: 4, item2: 6}})
```
