As you may expect, the `MAX` function retrieves the _largest_ value from a set of values. For example:

```sql
SELECT MAX(price)
FROM products;
```

This query looks through all of the prices in the `products` table and returns the price with the largest price value. Remember it only returns the `price`, not the rest of the record! You always need to specify each field you want a query to return.