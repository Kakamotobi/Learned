# Database

## Table of Contents
- [What is a Database?](#what-is-a-database)
- [Evolution of Databases](#evolution-of-databases)
- [Some Types of Databases](#some-types-of-databases)
  - [Hierarchical Database](#hierarchical-database)
  - [Relational Database](#relational-database)
  - [Non-Relational Database](#non-relational-database)
  - [Object-Oriented Database Management Systems(OODBMS)](#object-oriented-database-management-systemsoodbms)

## What is a Database?
> A database is an organized collection of structured information, or data, typically stored electronically in a computer system. A database is usually controlled by a database management system (DBMS). Together, the data and the DBMS, along with the applications that are associated with them, are referred to as a database system, often shortened to just database. | Oracle

- A DB also needs a query language to be able to manipulate data on the DB and perform CRUD operations.
  - In RDBMS, SQL is the most popular query language.
  - MongoDB (a popular NoSQL DB) uses the MongoDB Query Language(MQL).

## Evolution of Databases
- Historically, the concept of a "database" long existed before computers.
  - For example, data would be recorded in papers and stored in filing cabinets. This was very inefficient and unreliable.
  - As computers were invented, better data management became possible, and hence, databases were introduced.
- In 1964, Charles Bachman developed the first database called Integrated Data Store(IDS).
  - This was a **Navigational Database** based on a _network model_.
    - The _network model_ was a many-to-many relationship, which allows a data entry/record to have more than one parent and one child.
- In 1966, IBM released the Integrated Management Systems.
  - This is a DB system that is still being used today.
  - This was a Navigational Database based on a _hierarchical model_.
    - The _hierarchical model_ was a one-to-many relationship, which allows a data entry/record to have only one parent.
- In 1969, Edgar Codd theorized and released a paper on a new way to store/access data: the **Relational Database Model**.
  - This is a popular DB model being used today.
- In the mid-1970s, Stonebraker and Wong researching a relational database model called **Interactive Graphics and Retrieval System(INGRES)**, which used **QUEL** as the query language.
- In 1974, IBM developed **SQL** for their relational database called **System R**.
  - SQL became the standard language for DB queries.
- In the 1980s, Navigational DBs began to die out and instead, _Relational DBs loaded with SQL became the standard_.
- In the mid-1980s, **Object-Oriented Database Management Systems(OODBMS)** emerged.
- In the 1990s, OODBMS became more popular.
- In 1991, with the advent of the **WWW** and online businesses, the DB industry grew incredibly.
- In 1995, **MySQL** was created as an open-source project, which allowed alternative services other than big companies (Ex: Oracle, Microsoft).
- In 1998, **NoSQL** was coined.
- In the 2000s, NoSQL DBs grew in popularity as the internet grew more and unstructured data was prevalent.
- In the 2010s, the term "big data" began to trend, indicating an era of huge amounts of (private/public) data being collected and managed.
  - This meant the need for more secure DB management.
  - Also, through the impact of globalization, **Distributed Databases/Systems** began to take place.
- In recent times, **Cloud Databases** and **Self-Driving Databases** are breaking new ground.

## Some Types of Databases
### Hierarchical Database
- **A Hierarchical DB is a family tree-like structure.**
- Although the parent child structure can be very complex, it provides high performance in accessing/querying.
### Relational Database
- a.k.a. "**SQL DB**" because SQL is the most commonly used query language for Relational DBs.
  - _The terms SQL and Relational are used interchangeably since most Relational DBs use SQL._
- **A Relational DB stores data in tables, with rows and columns, that use _foreign keys_ to join with other tables.**
- It is called a "relational" DB because it shows the relationship between different data records (through the use of keys).
  - Ex: a User table that contains user information, and a PurchasedGames table that contains the games that the users have purchased.
- DB Ex: MySQL, Postgres, SQLite, Microsoft SQL Server, Oracle.
### Non-Relational Database
- a.k.a. "**NoSQL DB**".
  - _Refers to DBs that use query languages other than SQL to store/access data._
- **A Non-Relational DB stores unstructured data.**
- Non-Relational DBs grew as web applications became more complex and varying.
#### Types of Non-Relational Database
##### Document Database
- Data is stored in formats like JSON, BSON, XML documents.
- Documents can be nested.
- Data is close to how it is formatted in the application.
  - i.e. less time spent on formatting data.
- Use Case Ex: e-commerce, trading platforms, mobile app.
- DB Ex: MongoDB.
##### Key-Value Stores
- Data is stored as key-value pairs.
- It resembles an RDBMS with two columns (attribute name, value).
- Use Case Ex: shopping cart, user profile/preferences.
- DB Ex: Redis.
##### Column-Oriented Database
- Data is organized in columns.
  - cf. RDBMS goes row by row.
- Use Case Ex: analytics.
##### Graph Database
- Data is represented as a node with connections to other data (nodes).
- It is quick in searching the connections between data.
  - cf. RDBMS requires joining multiple tables.
- Use Case Ex: social networks.
### Object-Oriented Database Management Systems(OODBMS)
- **An OODBMS view data as objects and stores them on a DB server's disk.**
- OODBMS is intended to work well with object-oriented programming.
- DB Ex: MongoDB Realm.

## Reference
[What Is a Database | Oracle](https://www.oracle.com/database/what-is-database/)  
[History of DBMS - GeeksforGeeks](https://www.geeksforgeeks.org/history-of-dbms/)  
[The history of databases - ThinkAutomation](https://www.thinkautomation.com/histories/the-history-of-databases/#:~:text=The%201960s%20%E2%80%93%20beginnings&text=Charles%20Bachman%20designed%20the%20first,Integrated%20Data%20Store%2C%20or%20IDS.)  
[Types of Databases | MongoDB](https://www.mongodb.com/databases/types)  
