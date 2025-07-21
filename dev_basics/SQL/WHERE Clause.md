In order to keep learning about CRUD operations in SQL, we need to learn how to make the instructions we send to the database more specific. SQL accepts a `WHERE` statement within a query that allows us to be very specific with our instructions.

If we were unable to specify the record we wanted to `READ`, `UPDATE`, or `DELETE` making queries to a database would be very frustrating, and very inefficient.

## Using a WHERE Clause

Say we had _over 9000_ records in our `users` table. We often want to look at specific user data within that table without retrieving _all_ the other records in the table. We can use a `SELECT` statement followed by a `WHERE` clause to specify which records to retrieve. The `SELECT` statement stays the same, we just _add_ the `WHERE` clause to the end of the `SELECT`. Here's an example:

```sql
SELECT name FROM users WHERE power_level >= 9000;
```

This will select only the `name` field of any user within the `users` table `WHERE` the `power_level` field is greater than or equal to `9000`.