# MongoDB Learning Journey

Welcome to my MongoDB learning repository! This repo is a collection of notes, code snippets, and examples as I learn about MongoDB, focusing on creating databases, collections, documents, CRUD operations, indexing, and aggregation.

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Creating Databases and Collections](#creating-databases-and-collections)
4. [CRUD Operations](#crud-operations)
5. [Indexing](#indexing)
6. [Aggregation](#aggregation)
7. [Resources](#resources)
8. [Contributing](#contributing)
9. [License](#license)

## Introduction

This repository documents my journey in learning MongoDB. It includes my notes, code snippets, and various examples to illustrate key concepts and features of MongoDB, particularly focusing on creating databases, collections, documents, CRUD operations, indexing, and aggregation.

## Getting Started

To get started with MongoDB, follow these steps:

1. **Install MongoDB**: You can find the installation guide [here](https://docs.mongodb.com/manual/installation/).
2. **Set Up MongoDB**: Configure MongoDB to start as a service or run it manually from the command line.
3. **MongoDB Shell**: Learn the basics of the MongoDB shell to interact with your database.

## Creating Databases and Collections

MongoDB stores data in databases, which in turn have collections that store documents.

- **Create a Database**: Use the `use` command to switch to a database. If it doesn't exist, it will be created.
  ```javascript
  use myDatabase
  ```
- **Create a Collection**: Use the `createCollection` method.
  ```javascript
  db.createCollection("myCollection");
  ```

## CRUD Operations

CRUD stands for Create, Read, Update, and Delete operations. These are the basic operations you can perform on your data.

- **Create**: Insert documents into a collection.

  ```javascript
  db.myCollection.insertOne({ name: "John", age: 30 });
  db.myCollection.insertMany([
    { name: "Jane", age: 25 },
    { name: "Doe", age: 22 },
  ]);
  db.students.insertOne({name:"Aleagy", age:27, gpa:4.5,joined:new Date("2015-08-31"),friends:["Jane", "Lilian", "Jacob"],address:{street:"234 Nyamasaria str"},city:"Kisumu", zip:6578})
  ```
  - Documents can contain arrays, booleans, dates, and other nested documents.
  ````javascript

  db.myCollection.insertMany([
    {name:"Almond",
     age:30, 
     gpa:2.7,
     married: false, 
     joined: new Date("2018-02-15"), 
     left: null, 
     friends: ["Adryan", "Mercy", "Joel"], 
     address:{street:"123 Fake Str",
              city:"New York",
              zipcode: 12345}},
    {name:"Aleagy", 
    age:27, 
    gpa:4.5,
    married: true,
    joined:new Date("2015-08-31"),
    friends:["Jane", "Lilian", "Jacob"],
    address:{street:"234 Nyamasaria str",
             city:"Kisumu", 
             zip:6578}}])
  ````

- **Read**: Query documents from a collection.
  ```javascript
  db.myCollection.find({ name: "John" });
  db.myCollection.find().pretty();
  ```
- **Update**: Modify existing documents in a collection.
  ```javascript
  db.myCollection.updateOne({ name: "John" }, { $set: { age: 31 } });
  db.myCollection.updateMany(
    { age: { $lt: 25 } },
    { $set: { status: "young" } }
  );
  ```
- **Delete**: Remove documents from a collection.
  ```javascript
  db.myCollection.deleteOne({ name: "John" });
  db.myCollection.deleteMany({ age: { $lt: 25 } });
  ```

## Indexing

Indexes improve query performance. Learn how to create and use indexes.

- **Create Index**: Use the `createIndex` method.
  ```javascript
  db.myCollection.createIndex({ name: 1 });
  ```
- **View Indexes**: List all indexes in a collection.
  ```javascript
  db.myCollection.getIndexes();
  ```

## Aggregation

Aggregation operations process data records and return computed results. They are used for operations such as grouping and summarizing data.

- **Aggregation Example**:
  ```javascript
  db.myCollection.aggregate([
    { $match: { age: { $gte: 30 } } },
    { $group: { _id: "$age", total: { $sum: 1 } } },
  ]);
  ```
- **More Aggregation Operations**:
  - `$project`: Reshape each document in the stream.
  - `$sort`: Sort documents in the stream.
  - `$limit`: Limit the number of documents in the stream.
  - `$lookup`: Perform a left outer join to a collection in the same database.

## Resources

- [MongoDB Official Documentation](https://docs.mongodb.com/)
- [MongoDB University](https://university.mongodb.com/)
- [MongoDB GitHub](https://github.com/mongodb/mongo)

## Contributing

If you find any issues or want to contribute to this repository, feel free to open an issue or submit a pull request. Contributions are welcome!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
