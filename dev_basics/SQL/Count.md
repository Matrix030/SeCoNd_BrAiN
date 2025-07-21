We can use a `SELECT` statement to get a _count_ of the records within a table. This can be very useful when we need to know _how many_ records there are, but we don't particularly care what's in them.

Here's an example in SQLite:

```sql
SELECT COUNT(*) FROM employees;
```

The `*` in this case refers to a column name. We don't care about the count of a _specific column_ - we want to know the number of _total records_ so we can use the wildcard (*).