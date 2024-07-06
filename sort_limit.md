# Sort
- [ ] Specify the order of the documents returned
```javascript
db.students.find().sort({name: 1}) //sort in alphabetical order

db.students.find().sort({name: -1}) // reverse alphabetical order

db.students.find().sort({age: 1}) // from lowest age

db.students.find().sort({age: -1}) //from the highest age
```

### Sorting by Multiple Fields
- [ ] You can also sort by multiple fields. For example, to sort by grade in ascending order and then by age in descending order:

```javascript
db.students.find().sort({ gpa: 1, age: -1 })
```

### Using Sorting in an Aggregation Pipeline
- [ ] In the aggregation framework, you can use the $sort stage to sort documents

```javascript
db.students.aggregate([
  { $sort: { age: 1 } }
])
```
# Limit
- [ ] Limit the output of the returned documents

```javascript
db.students.find().sort({name: 1}).limit(1) //first student in alphabetical order

db.students.find().sort({name: -1}).limit(1) //last student in alphabetical order

db.students.find().sort({age: 1}).limit(1) //youngest student

db.students.find().sort({age: -1}).limit(1) //oldest student
```

### Using limit in an Aggregation Pipeline

- [ ] In an aggregation pipeline, you can use the $limit stage to limit the number of documents.

```javascript
db.students.aggregate([
  { $sort: { age: 1 } },
  { $limit: 2 }
])

```