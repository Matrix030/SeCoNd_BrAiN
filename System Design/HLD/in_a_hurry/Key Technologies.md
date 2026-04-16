---
tags: [hld, system-design, database, sql, nosql]
aliases: ["HLD Key Technologies"]
---

# Key Technologies

Core databases and storage technologies to know for system design interviews.

---

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
> 1) While NoSQL databases are great for handling unstructured data, relational databases can also have JSON columns with flexible schemas.
> 2) NoSQL databases are great for scaling horizontally, relational databases can also scale horizontally with the right architecture. When you're discussing NoSQL databases in your system design interview, make sure you're not making broad statements but instead discussing the specific features of the database you're using and how they will help you solve the problem at hand.

> [!info]
> 1) **Data Models:** NoSQL databases come in many different flavors, each with its own data model. The most common types of NoSQL databases are key-value stores, document stores, column-family stores, and graph databases.
> 2) **Consistency Models:** NoSQL databases offer various consistency models ranging from strong to eventual consistency. Strong consistency ensures that all nodes in the system have the same data at the same time, while eventual consistency ensures that all nodes will eventually have the same data.
> 3) **Indexing:** Just like with relational databases, NoSQL databases support indexing to make data faster to query. The most common types of indexes are B-Tree and Hash Table indexes.
> 4) **Scalability:** NoSQL databases scale horizontally by using [consistent hashing](http://highscalability.com/blog/2023/2/22/consistent-hashing-algorithm.html#:~:text=Consistent%20hashing%20is%20a%20distributed,of%20nodes%20changes%20%5B4%5D.) and/or [sharding](https://www.mongodb.com/features/database-sharding-explained#:~:text=Sharding%20is%20a%20method%20for,storage%20capacity%20of%20the%20system.) to distribute data across many servers.

Common NoSQL databases - DynamoDB, Cassandra and MongoDB.

## Blob Storage

##### When to use Blob Storage:
1) To store large, unstructured blobs of data. This could be images, videos or other files.
2) Storing these large blobs in traditional database is both expensive and inefficient and should be avoided when possible.
3) You can upload a blob of data and that data is stored and get back a URL.
4) You can then use this URL to download the blob of data.
5) Often times blob storage services work in conjunction with CDNs, for faster downloads form anywhere in the world. Using CDNs to cache the file/blob in edge locations around the world.

> [!warning]
> Avoid using blob storage like S3 as your primary database unless you have a very good reason. In a typical setup you will have a core database like Postgres or DynamoDB that has pointers (just a url) to the blobs stored in S3. This allows you to use the database to query and index the data with very low latency, while still getting the benefits of cheap blob storage.

A very common setup when dealing with large binary artifacts looks like this:
![[Pasted image 20260415231611.png]]

To upload:
- When clients want to upload a file, they request a presigned URL from the server.
- The server returns a presigned URL to the client, recording it in the database.
- The client uploads the file to the presigned URL.
- The blob storage triggers a notification to the server that the upload is complete and the status is updated.

To download:
- The client requests a specific file from the server and are returned a presigned URL.
- The client uses the presigned URL to download the file via the CDN, which proxies the request to the underlying blob storage.

> [!info]
> 1) **Durability**: Blob storage services are designed to be incredibly durable. They use techniques like replication and erasure coding to ensure that your data is safe even if a disk or server fails.
> 2) **Scalability**: Hosted blob storage solutions like AWS S3 can be considered infinitely scalable. They can store an unlimited amount of data and can handle an unlimited number of requests (obviously within the limits of your account).
> 3) **Cost**: Blob storage services are designed to be cost effective. They are much cheaper than storing large blobs of data in a traditional database. For example, AWS S3 charges $0.023 per GB per month for the first 50 TB of storage. This is much cheaper than storing the same data in a database like DynamoDB, which charges $1.25 per GB per month for the first 10 TB of storage.
> 4) **Security**: Blob storage services have built-in security features like encryption at rest and in transit. They also have access control features that allow you to control who can access your data.
> 5) **Upload and Download Directly from the Client**: Blob storage services allow you to upload and download blobs directly from the client. This is useful for applications that need to store and retrieve large blobs of data, like images or videos. presigned URLs can be used to grant temporary access to a blob -- either for upload or download.
> 6) **Chunking**: When uploading large files, it's common to use chunking to upload the file in smaller pieces. This allows you to resume an upload if it fails partway through, and it also allows you to upload the file in parallel. This is especially useful for large files, where uploading the entire file at once might take a long time. Modern blob storage services like S3 support chunking out of the box via the [multipart upload API](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html).

##### Examples of blob storage services
1) Amazon S3
2) Google Cloud Storage
3) Azure Blob

## Search Optimized Database

##### What is a search optimized database and when should you use it?

1) Use when you are tasked with implementing a full-text search as a feature of your design.
2) Full-text search is the ability to search through a large amount of text data and find relevant results.
3) This is different from a traditional database query, which is usually based on exact matches or ranges.

Without a search optimized database, you would need to run a query that looks something like this:

``` sql
SELECT * FROM documents WHERE document_text LIKE '%search_term%'
```

This query is slow and inefficient, and it doesn't scale well because it requires a full table scan. That means the database has to grab each record and test it against your predicate rather than relying on an index or lookup. Which is slow

> [!info]
> Search optimized databases, on the other hand, are specifically designed to handle full-text search. They use techniques like indexing, tokenization, and stemming to make search queries fast and efficient. In short, they work by building what are called [inverted indexes](https://www.hellointerview.com/learn/system-design/deep-dives/elasticsearch#lucene-segment-features). Inverted indexes are a data structure that maps from words to the documents that contain them. This allows you to quickly find documents that contain a given word. A simple example of an inverted index might look like this:

``` json
{
  "word1": [doc1, doc2, doc3],
  "word2": [doc2, doc3, doc4],
  "word3": [doc1, doc3, doc4]
}
```

Now, instead of scanning the entire table, the database can quickly look up the word in the query and find all the matching documents. Which is Fast!

Examples of search optimized databases are straightforward, consider an application like Ticketmaster that needs to search through a large number of events to find relevant results. Or a social media platform like Twitter that needs to search through a large number of tweets to find relevant results. In either case, a search optimized database would be an optimal choice.


> [!info]
> 1) **Inverted Indexes**: As just mentioned, search optimized databases use inverted indexes to make search queries fast and efficient. An inverted index is a data structure that maps from words to the documents that contain them. This allows you to quickly find documents that contain a given word.
> 2) **Tokenization**: Tokenization is the process of breaking a piece of text into individual words. This allows you to map from words to documents in the inverted index.
> 3) **Stemming**: Stemming is the process of reducing words to their root form. This allows you to match different forms of the same word. For example, "running" and "runs" would both be reduced to "run".
> 4) **Fuzzy Search**: Fuzzy search is the ability to find results that are similar to a given search term. Most search optimized databases support fuzzy search out of the box as a configuration option. In short, this works by using algorithms that can tolerate slight misspellings or variations in the search term. This is achieved through techniques like edit distance calculation, which measures how many letters need to be changed, added, or removed to transform one word into another.
> 5) **Scaling**: Just like traditional databases, search optimized databases scale by adding more nodes to a cluster and sharding data across those nodes.

##### Examples of search optimized databases
1) Elastic Search
2) Postgres with GIN indexes
3) Redis - tho bad, has full text search capability

