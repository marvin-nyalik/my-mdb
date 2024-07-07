# Aggregation
- Aggregation is a way of processing a large number of documents in a collection by means of passing them `through a pipeline of stages`. Each stage performs an operation on the input documents and passes the result to the next stage. These stages can filter, group, sort, reshape, and modify documents that pass through the pipeline. The most common use case for aggregation is to perform complex queries and data transformations to produce computed results.

### Aggregation Pipeline
The aggregation pipeline consists of stages through which documents pass in sequence. Each stage transforms the documents as they pass through. Common stages include:

1. **`$match`**: Filters documents to pass only those that match specified conditions. It's similar to the find operation in queries.

```javascript
db.students.aggregate([
  {
    $match: {
      fullTime: true,
      grade: { $in: ["A", "B"] }
    }
  }
])
```

2. **`$group`**:  The `$group` stage in MongoDB's aggregation framework is used to group documents by a specified identifier expression and to apply accumulator expressions, such as `$sum`, `$avg`, `$min`, `$max`, etc., to each group of documents..

```javascript
db.students.aggregate([
  {
    $group: {
      _id: "$grade",
      averageAge: { $avg: "$age" },
      totalStudents: { $sum: 1 }
    }
  }
])
```

