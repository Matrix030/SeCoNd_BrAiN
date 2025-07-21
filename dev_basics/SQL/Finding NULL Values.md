You can use a `WHERE` clause to filter values by whether or not they're `NULL`.

## IS NULL

```sql
SELECT name FROM users WHERE first_name IS NULL;
```

## IS NOT NULL

```sql
SELECT name FROM users WHERE first_name IS NOT NULL;
```