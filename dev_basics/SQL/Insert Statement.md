Tables are pretty useless without data in them! In SQL we can add records to a table using an `INSERT INTO` statement. When using an `INSERT` statement we must first specify the `table` we are inserting the record into, followed by the `fields` within that table we want to add `VALUES` to.

Example `INSERT INTO` statement:

```sql
INSERT INTO employees(id, name, title)
VALUES (1, 'Allan', 'Engineer');
```