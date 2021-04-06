---
title: "Database Families"
date: 2018-08-12
draft: false
description: "A brief overview of different families of databases"
tags: ['sql', 'database', 'nosql']
categories: ['database']
toc: true

---

This article isn't about SQL vs NoSQL. It's just general overview of different types of databases. 

## SQL

SQL databases are similar to excel spread sheets. They use tables to organize data and each table uses columns and rows. Data stored in SQL databases is normalized, meaning duplicate data isn't stored. The tables use indexes on columns to speed up search operations. And foreign keys are used to join tables together to build complex relationships. There are many SQL databases options available, MySQL, PostgreSQL, SQLitght, and Amazon Auroa are popular SQL databases. 

## NoSQL

NoSQL databases do not require data to be stored in columns and rows. Instead data is stored as more of an object. For example, in a users table one user may have a field called address, and another user may have a field called phone number. The users are not required to have the same fields as in an SQL database. Because of this flexibility big data solutions rely on NoSQL databases because the database schema is not known ahead of time. Another reason NoSQL is popular for big data is because NoSQL database scale horizontally very well.  However, there are now more options for SQL database that scale horizontally, like [CockroachDB](https://www.cockroachlabs.com/) . One downside to NoSQL databases is the lack ability to join tables. If you wanted a table join, you would have to write application code to create the join.

## Key/Value Stores

A key value store is pretty much what is sounds like. It's a database designed to map keys to values. The values may be string, integers, arrays, etc. Memcached, Redis, and Amazon's DynamoDB are all key/value store databases. Key value stores are often used for caching layers. But they can model more complex data as well. However, the techniques for this may seems somewhat unorthodox. Often the keys will be given names that contain multiple pieces of information like `user:gold:active` This allows filtering the data based on key rather than the value. Key value stores are popular when fast lookup is important. 

```json
"234193:user:active": { name: "John", email: "john@example.com"}
```

## Document Database

Mongodb and CouchDB are document databases. They allow you to model data as whole units. So if you wanted to model a blog post for example, it is possible to store the title, content, and comments in the same entry. This allows you to retrieve everything in one query. 

```js
"09350333": {
    author: "John",
    date: "01-01-2018",
    title: "A Good Day",
    body: "Today was a good day...",
    comments: {
        1: "Great post!",
        2: "Maybe someday for me too."
    }
}
```

## Column Store Database 

A column store database is often used for really big data. The data is written to disk in a special way allowing it to be read without retrieving extra data. So a single column in a table can be read, while the other columns are ignored. Google's Big Table is a column store database. 

## Graph Databases

Graph databases as the name might imply are used to store complex relationships. They are perfect for creating social networks or other relationships that are best expressed as a graph. Here are a few options for graph databases:  [Neo4j](https://neo4j.com/),  [Dgraph](https://dgraph.io/), [Amazon Neptune](https://aws.amazon.com/neptune/) . There are different query languages for graph databases. 

https://www.3pillarglobal.com/insights/exploring-the-different-types-of-nosql-databases

https://highlyscalable.wordpress.com/2012/03/01/nosql-data-modeling-techniques/
