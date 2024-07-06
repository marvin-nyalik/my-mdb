# Projection
In MongoDB, projection is used to specify or restrict the fields to be returned in the documents that match a query. This is useful for improving performance and reducing the amount of data transferred over the network. You can use projection to include or exclude specific fields in the query results.

### Basic Projection
To perform projection, you use the second parameter of the find method. The projection document specifies the fields to include or exclude.

```javascript
[
  { "name": "Alice", "age": 24, "grade": "B", "address": "123 Main St" },
  { "name": "Bob", "age": 22, "grade": "A", "address": "456 Elm St" },
  { "name": "Charlie", "age": 23, "grade": "C", "address": "789 Oak St" },
  { "name": "David", "age": 25, "grade": "B", "address": "101 Pine St" },
  { "name": "Eva", "age": 21, "grade": "A", "address": "202 Maple St" }
]

```

#### Including Specific Fields
- [ ] To include specific fields, set the field to `1` or `true` in the projection document. The _id field is included by default, but you can exclude it by setting it to `0` or `false`.

```javascript
// Include only the name and age fields
db.students.find({}, { name: 1, age: 1 })

// Include only the name and age fields, exclude _id
db.students.find({}, { name: 1, age: 1, _id: 0 })
```

### Using Projection in Aggregation
In an aggregation pipeline, you use the $project stage to specify fields.

```javascript
db.students.aggregate([
  { $project: { name: 1, age: 1, _id: 0 } }
])
```

### Example with limit and sort

```javascript
//retrieves documents from the students collection, includes only the name and age fields (excluding _id), sorts the results by name in ascending order and age in descending order, and limits the output to 2 documents

db.students.find({}, { name: 1, age: 1, _id: 0 }).skip(1).sort({ name: 1, age: -1}).limit(5)

//output
[
  { "name": "Alice", "age": 24 },
  { "name": "Bob", "age": 22 }
]

```