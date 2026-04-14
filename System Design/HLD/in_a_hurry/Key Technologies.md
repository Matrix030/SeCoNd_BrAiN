## Core Database

The most common databases:
- Relational (Postgres)
- NoSQL (DynamoDB)

### Relational Databases

##### What is a relational database and when should you use it?

Postgres, MySQL
- They're often used for transactional data (e.g. user records, order records, etc) and are typically the default choice for a product design
- Relational databases store your data in tables, which are composed of rows and columns.
- Each row represents a single record, and each column represents a single field on that record.
- A users table might have a name column and an email column.
- Relational databases are often queried using SQL, a declarative language for querying data.

Beyond simply storing data, relational databases come equipped with several features which are useful for system design interviews. The most important of these are:
1) SQL Joins:
	- Joins are a way of combining data from multiple tables.
	- This is important for querying data and SQL databases can support arbitrary joins between tables.
	- Joins can be also a **major performance bottleneck** in your system so minimize them where possible.
2) Indexes:
	- Indexes are a way of storing data in a way that makes it faster to query. Example, if you ahve a `users` table with a `name` column, you might create an index on the `name` column. This would allow you to query for users by name much faster than if you didn't have an index.
	- Indexes are often implemented using a `B-Tree` or a `Hash Table`.
	- RL DBs have support for arbitrarily many indexes, which allows you to optimize for different queries
	- Their support for multi-column and specialized indexes (e.g. geospatial indexes, full-text indexes)
3) RDBMS Transactions: 
	- Transactions are a way of grouping multiple operations together into a single atomic operation. For example, if you have a `users` table and a `posts` table, you might want to create a new user and a new post for hta user at the same time. If you do this in a transaction, either both operations will succeed or both will fail.
	- This ensures you don't have invalid data like a post from a user who doesn't exist


### NoSQL Databases

- NoSQL databases are a broad category of databases designed to accommodate a wide range of data models, including key-value, column-family, and graph formats.
- Unlike relational databases, NoSQL databases do not use a traditional table-based structure and are often schema-less.
- This flexibility allows NoSQL databases to handle large volumes of unstructured, semi-structured, or structured data, and to scale horizontally with ease.
![[Pasted image 20260413135102.png]]

NoSQL DBs are strong candidates for situations where:
- **Flexible Data Models**: Your data model is evolving or you need to store different types of data structures without a fixed schema
- **Scalablility**: Your application needs to scale horizontally (across many servers) to accommodate large amounts of data or high user loads
- **Handling Big Data and Real-Time Web Apps**: You have applications dealing with large volumes of data, especially unstructured data, or applications requiring real-time data processing and analytics.

> [!warning]
>  While NoSQL databases are great for handling unstructured data, relational databases can also have JSON columns with flexible schemas.
 NoSQL databases are great for scaling horizontally, relational databases can also scale horizontally with the right architecture. When you're discussing NoSQL databases in your system design interview, make sure you're not making broad statements but instead discussing the specific features of the database you're using and how they will help you solve the problem at hand.
