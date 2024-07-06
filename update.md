# Update

- In MongoDB, you can update documents using the updateOne, updateMany, or findOneAndUpdate methods. Here’s how you can use these methods to update documents in the students collection.

```javascript
db.students.insertMany(
[
  { "name": "Alice", "age": 24, "grade": "B", "address": "123 Main St" },
  { "name": "Bob", "age": 22, "grade": "A", "address": "456 Elm St" },
  { "name": "Charlie", "age": 23, "grade": "C", "address": "789 Oak St" },
  { "name": "David", "age": 25, "grade": "B", "address": "101 Pine St" },
  { "name": "Eva", "age": 21, "grade": "A", "address": "202 Maple St" }
])

```

### Update One Document
To update a single document, use updateOne. For example, to update Alice's grade to "A":

```javascript
db.students.updateOne(
  { name: "Alice" },
  { $set: { grade: "A" } }
)

```

### Update Multiple Documents
To update multiple documents, use updateMany. For example, to update the grade of all students living on "Elm St" to "B":

```javascript
db.students.updateMany(
  { address: /Elm St$/ },
  { $set: { grade: "B" } }
)

```

### Find One and Update
To find one document and update it, returning the updated document, use findOneAndUpdate. For example, to find Bob and update his age to 23, returning the updated document.

```javascript
db.students.findOneAndUpdate(
  { name: "Bob" },
  { $set: { age: 23 } },
  { returnDocument: "after" }
)

```
## Complex Update

```javascript
db.students.updateMany(
  {
    $and: [
      { $or: [{ age: { $gt: 22 } }, { grade: "A" }] },
      { fullTime: { $exists: true, $eq: true } }
    ]
  },
  { $set: { leavingDate: null } }
)
```
