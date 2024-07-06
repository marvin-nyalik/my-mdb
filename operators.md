# Operators for Complex Queries

MongoDB supports a variety of operators for creating complex queries. 

All operators are preceeded by `$` sign

Here are examples of how to use these operators, including:
- `$lt` (less than), 
- `$lte` (less than or equal to), 
- `$gt` (greater than), 
- `$gte` (greater than or equal to), 
- `$eq` (equal), 
- `$ne` (not equal), 
- `$in` (in array), 
- `$nin` (not in array),
- `$and`, `$or`, `$not`, and `$exists.`

### Consider the bellow dataset
```javascript
[
  { "name": "Alice", "age": 24, "grade": "B", "address": "123 Main St", "fullTime": true },
  { "name": "Bob", "age": 22, "grade": "A", "address": "456 Elm St", "fullTime": true },
  { "name": "Charlie", "age": 23, "grade": "C", "address": "789 Oak St", "fullTime": false },
  { "name": "David", "age": 25, "grade": "B", "address": "101 Pine St" },
  { "name": "Eva", "age": 21, "grade": "A", "address": "202 Maple St", "fullTime": true }
]

```

## Basic Operators
#### Less Than ($lt)/ ($lte)
Find students younger than 23 years:

```javascript
db.students.find({ age: { $lt: 23 } })
```

#### Greater Than ($$gt) / ($gte)
Find students older than 23 years:

```javascript
db.students.find({ age: { $gt: 23 } })
```

#### Range of Values
Find students aged between 22 and 27 years

```javascript
db.students.find({ age: { $gte: 22, $lte: 27 } })
```

#### Equal ($eq)
Find students aged 22 years:

```javascript
db.students.find({ age: { $eq: 22 } })
```

#### Not Equal ($ne)
Find students not aged 22 years:

```javascript
db.students.find({ age: { $ne: 22 } })
```

## Array Operators
#### In ($in)
Find students whose grade is either "A" or "B":

```javascript
db.students.find({ grade: { $in: ["A", "B"] } })
```

#### Not In ($nin)
Find students whose grade is neither "A" nor "B":

```javascript
db.students.find({ grade: { $nin: ["A", "B"] } })
```

## Logical Operators
#### And ($and)
Find students older than 22 and whose grade is "A":

```javascript
db.students.find({ $and: [{ age: { $gt: 22 } }, { grade: "A" }] })
```

#### Or ($or)
Find students younger than 23 or whose grade is "A":

```javascript
db.students.find({ $or: [{ age: { $lt: 23 } }, { grade: "A" }] })
```

#### NOr ($nor)
Find students neither younger than 23 nor with grade "A":
- Selects the documents that fail all the expressions in the array i.e `Dont match` any specified conditions
```javascript
db.students.find({ $nor: [{ age: { $lt: 23 } }, { grade: "A" }] })
```

#### Not ($not)
Find students who are not older than 23 years:

```javascript
db.students.find({ age: { $not: { $gt: 23 } } })
```

## Element Operators
#### Exists ($exists)
Find students who have the fullTime field:

```javascript
db.students.find({ fullTime: { $exists: true } })
```

#### Type ($type)
Find students where the fullTime field is of type boolean:

```javascript
db.students.find({ fullTime: { $type: "boolean" } })
```

## Combination Example
Find students who are either older than 22 or whose grade is "A", and who are also full-time students:

```javascript
db.students.find({ $and: [
  { $or: [{age: { $gt: 22 }}, {grade: "A"}]},
  { fullTime: {$exists: true, $eq: true}}
]})
```