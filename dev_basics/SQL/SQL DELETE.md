When a user deletes their account on Twitter, or deletes a comment on a YouTube video, that data needs to be removed from its respective database.

## DELETE Statement

A `DELETE` statement removes all records from a table that match the `WHERE` clause. As an example:

```sql
DELETE FROM employees
    WHERE id = 251;
```

This `DELETE` statement removes all records from the `employees` table that have an id of `251`!