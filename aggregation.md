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

2. **`$group`**:  The `$group` stage in MongoDB's aggregation framework is used to group documents by a specified identifier expression and to apply accumulator expressions, such as `$sum`, `$avg`, `$min`, `$max`, `$push`, `$addToSet`, `$first`, `$last` etc., to each group of documents..

```javascript
db.students.aggregate([
  {
    $group: {
      _id: "$grade",
      averageAge: { $avg: "$age" },
      totalStudents: { $sum: 1 },
      oldest: { $max: "$age"}
    }
  }
])

// field paths vs Expressions vs Literal values
db.sales.aggregate([
  {
    $group: {
      _id: "$productId",
      totalQuantity: { $sum: 100 },   // Literal value
      averagePrice: { $avg: "$price" },       // Field path
      minPrice: { $min: "$price" },           // Field path
      maxPrice: { $max: "$price" },           // Field path
      totalRevenue: { $sum: { $multiply: ["$price", "$quantity"] } }, // Expression
    }
  }
]);
```

3. `$project`: Reshapes each document in the stream, such as by adding, removing, or renaming fields.

```javascript
db.sales.aggregate([
  {
    $group: {
      _id: "$productId",
      totalQuantity: { $sum: "$quantity" },
      averagePrice: { $avg: "$price" },
      minPrice: { $min: "$price" },
      maxPrice: { $max: "$price" },
      totalRevenue: { $sum: { $multiply: ["$price", "$quantity"] } }
    }
  },
  {
    $project: {
      productId: "$_id",
      totalQuantity: 1,
      averagePrice: 1,
      minPrice: 1,
      maxPrice: 1,
      totalRevenue: 1,
      status: "checked"
    }
  }
]);
```

4. `$sort`: Sorts all input documents and returns them to the pipeline in sorted order.

```javascript
db.sales.aggregate([
  {
    $sort: {
      totalQuantity: -1
    }
  }
])
```

5. `$limit` and `$skip`, both take integers

```javascript
db.sales.aggregate([
  { $limit: 5 }
  { $skip: 10 }
])
```

6. `$unwind`: This stage in MongoDB's aggregation pipeline is used to deconstruct an array field from the input documents to output a document for each element of the array. This is particularly useful when you have documents with arrays and you want to process or analyze each element of the array individually.

Assume we have a sales collection:
```javascript
db.sales.insertOne({
  "_id": 1,
  "orderId": 101,
  "items": [
    { "productId": 1, "quantity": 10, "price": 100 },
    { "productId": 2, "quantity": 5, "price": 200 }
  ]
}
)
``` 

We want to unwind the items array so that each item becomes its own document, then group by productId to calculate total quantities sold, and finally sort by totalQuantity in descending order.

```javascript
db.sales.aggregate([
  // Stage 1: Unwind the items array
  {
    $unwind: "$items"
  },
  // Stage 2: Group by productId
  {
    $group: {
      _id: "$items.productId",
      totalQuantity: { $sum: "$items.quantity" },
      averagePrice: { $avg: "$items.price" }
    }
  },
  // Stage 3: Project the fields
  {
    $project: {
      productId: "$_id",
      totalQuantity: 1,
      averagePrice: 1
    }
  },
  // Stage 4: Sort by totalQuantity in descending order
  {
    $sort: {
      totalQuantity: -1
    }
  }
]);
```

7. `$lookup`: The `$lookup` stage in MongoDB's aggregation framework performs a left outer join to another collection in the same database to filter in documents from the "joined" collection for processing. This is useful for combining data from multiple collections. Take in `from:"collection", localField:"localField", foreignField:"fromFromColl", as:"resltArray"`

#### orders collection

```javascript
[
  { "_id": 1, "orderId": 101, "customerId": 1, "amount": 500 },
  { "_id": 2, "orderId": 102, "customerId": 2, "amount": 300 },
  { "_id": 3, "orderId": 103, "customerId": 1, "amount": 700 }
]
```

#### customers collection

```javascript
[
  { "_id": 1, "customerId": 1, "name": "John Doe" },
  { "_id": 2, "customerId": 2, "name": "Jane Smith" },
  { "_id": 3, "customerId": 3, "name": "David Brown" }
]
```
We want to join the orders collection with the customers collection to add customer details to each order.

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",            // The target collection
      localField: "customerId",     // Field from the orders collection
      foreignField: "customerId",   // Field from the customers collection
      as: "customerDetails"         // Output array field - Array of documents from the customers collection
    }
  },
  {
    $unwind: "$customerDetails"     // Deconstructs the array created by $lookup
  },
  {
    $project: {
      orderId: 1,
      amount: 1,
      "customerDetails.name": 1,   // Include specific fields from the joined document customerDetails { "name": "Jane Smith"}  
      customerAddress: "$customerDetails.address", //If address existed, you can rename it like this
    }
  }
]);
```

8. **Mathematical Operators**:

```javascript
{ $add: [ "$quantity", 10 ] }
{ $subtract: [ "$price", "$discount" ] }
{ $multiply: [ "$price", "$quantity" ] }
{ $divide: [ "$total", "$quantity" ] }
{ $mod: [ "$price", 10 ] }
```

9. `$cond`: The $cond operator in MongoDB's aggregation framework allows you to implement conditional logic within your aggregation pipeline. It evaluates a boolean expression and returns one of two specified expressions depending on whether the boolean expression evaluates to true or false.

```javascript
db.orders.aggregate([
  {
    $project: {
      orderId: 1,
      amount: 1,
      category: {
        $cond: {
          if: { $gte: ["$amount", 500] },   // If amount is greater than or equal to 500
          then: "High",                     // Assign "High" category
          else: "Low"                       // Otherwise, assign "Low" category
        }
      }
    }
  }
]);
```

10. `$ifNull`: The `$ifNull` operator in MongoDB's aggregation framework is used to replace null values with a specified alternative value. This is particularly useful when you want to handle cases where a field might be missing or have a null value and you need to provide a default or substitute value.

```javascript
db.orders.aggregate([
  {
    $project: {
      orderId: 1,
      amount: 1,
      discount: { $ifNull: ["$discount", 0] }   // Replace null with 0 for discount
    }
  }
]);
```

11. `$out`: The $out stage in MongoDB's aggregation pipeline does not produce an output file directly. Instead, it writes the results of the aggregation pipeline to a new collection. This new collection can then be exported using the mongoexport tool.

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "customerId",
      as: "customerDetails"
    }
  },
  {
    $unwind: "$customerDetails"
  },
  {
    $project: {
      orderID: "$_id",
      orderId: 1,
      amount: 1,
      customerName: "$customerDetails.name",
      customerAddress: "$customerDetails.address"
    }
  },
  {
    $addFields: {
      discount: { $cond: { if: { $eq: ["$discount", null] }, then: 0, else: "$discount" } }
    }
  },
  {
    $out: "orders_aggregated" // Specify the name of the new collection to write the results to
  }
]);
```