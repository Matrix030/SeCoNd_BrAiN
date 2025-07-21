The `SUM` aggregation function returns the sum of a set of values.

For example, the query below returns a single record containing a single field. The returned value is equal to the _total_ salary being collected by all of the `employees` in the `employees` table.

```sql
SELECT SUM(salary)
FROM employees;
```

Which returns:

|SUM(SALARY)|
|---|
|2483|