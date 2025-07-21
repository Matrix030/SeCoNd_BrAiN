Sometimes we don't want to retrieve _every_ record from a table. For example, it's common for a production database table to have millions of rows, and `SELECT`ing all of them might crash your system! _The `LIMIT` keyword has entered the chat._

The `LIMIT` keyword can be used at the end of a select statement to reduce the number of records returned.

```sql
SELECT * FROM products
    WHERE product_name LIKE '%berry%'
    LIMIT 50;
```

The query above retrieves all the records from the `products` table where the name contains the word _berry_. If we ran this query on the Amazon database, it would almost certainly return a _lot_ of records.

The `LIMIT` statement only allows the database to return _up to_ 50 records matching the query. This means that if there aren't that many records matching the query, the `LIMIT` statement will not have an effect.