# Index
- Indexes in MongoDB improve the performance of search queries by allowing the database to quickly locate documents. 
- Indexes are special data structures that store a small portion of the data set in an `easy-to-traverse` form. Here’s a detailed guide on indexing in MongoDB, including creating, using, and managing indexes.

#### Creating Indexes
Indexes can be created on one or more fields in a collection. The basic syntax for creating an index is. 1/-1 specifies how to organize data in the index in ascending/descending order respectively.

```javascript
db.students.createIndex({name: 1})
```
when you run :
```javascript
db.students.find({name:"Larry"}).explain("executionStats")
```
after creating the index, `executionStages.docsExamined` will be way lower than its value prior to creating the index.

#### Compound Index
the command 
```javascript 
db.students.createIndex({ age: 1, grade: -1 })
```
 creates a single compound index in MongoDB. This compound index includes both the age field and the grade field, with age indexed in ascending order (1) and grade indexed in descending order (-1).

Compound indexes can improve the performance of queries that involve both fields. For example, the index created by this command would be beneficial for queries that filter or sort by both age and grade. It does not create two separate indexes for each field; instead, it combines them into one index.

#### Unique Index
```javascript
db.students.createIndex({name: 1}, {unique: true})
```

#### Text Index
To create a text index on a collection, you can use the createIndex method and specify the fields to be indexed with the text type. For example, if you have a posts collection and you want to create a text index on the title and content fields, you would do it like this

```javascript
db.posts.createIndex({ title: "text", content: "text" })
```

##### Text Search
Once a text index is created, you can use the $text operator to perform text searches. For example, to search for documents that contain the word "MongoDB" in either the title or content fields, you can run the following query:

```javascript
db.posts.find({ $text: { $search: "MongoDB" } })
```

#### TTL Index (Time to Live):

This creates a TTL index on the createdAt field, which automatically deletes documents after 1 hour (3600 seconds).

```javascript
db.students.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```

## Get All indexes

```javascript
db.students.getIndexes()
```

### Drop an Index

```javascript
db.students.dropIndex('index_name')
```

## Analyse index usage
This command provides statistics about index usage.

```javascript
db.students.aggregate([
  { $indexStats: {} }
])
```