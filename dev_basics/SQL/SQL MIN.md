The `MIN` function works the same as the `MAX` function but finds the _lowest_ value instead of the _highest_ value.

```sql
SELECT product_name, MIN(price)
FROM products;
```

This query returns the `product_name` and the `price` fields of the record with the lowest `price`.