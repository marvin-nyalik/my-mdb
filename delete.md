# Delete

- To delete documents in MongoDB, you can use deleteOne, deleteMany, or findOneAndDelete methods. Below are examples of how to use these methods to delete documents based on complex queries.

```javascript
[
  { "name": "Alice", "age": 24, "grade": "B", "address": "123 Main St", "fullTime": true },
  { "name": "Bob", "age": 22, "grade": "A", "address": "456 Elm St", "fullTime": true },
  { "name": "Charlie", "age": 23, "grade": "C", "address": "789 Oak St", "fullTime": false },
  { "name": "David", "age": 25, "grade": "B", "address": "101 Pine St" },
  { "name": "Eva", "age": 21, "grade": "A", "address": "202 Maple St", "fullTime": true }
]
```

#### Delete One Document
To delete a single document that matches the criteria
```javascript
db.students.deleteOne(
  {
    $and: [
      { $or: [{ age: { $gt: 22 } }, { grade: "A" }] },
      { fullTime: { $exists: true, $eq: true } }
    ]
  }
)
```

#### Delete Multiple Documents
To delete multiple documents that match the criteria:

```javascript
db.students.deleteMany(
  {
    $and: [
      { $or: [{ age: { $gt: 22 } }, { grade: "A" }] },
      { fullTime: { $exists: true, $eq: true } }
    ]
  }
)
```

#### FindOneAndDelete
The findOneAndDelete operation in MongoDB is useful when you want to delete a single document that matches a specified filter and get the deleted document back as the result. This operation is atomic: it retrieves the document and then deletes it, ensuring no other operation can modify the document between these two operations

```javascript
db.students.findOneAndDelete(
  {
    $and: [
      { $or: [{ age: { $gt: 22 } }, { grade: "A" }] },
      { fullTime: { $exists: true, $eq: true } }
    ]
  }
)
```
