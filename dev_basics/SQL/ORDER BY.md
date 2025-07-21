SQL also offers us the ability to sort the results of a query using `ORDER BY`. By default, the `ORDER BY` keyword sorts records by the given field in ascending order, or `ASC` for short. However, `ORDER BY` does support descending order as well with the keyword `DESC`.

## Examples

This query returns the `name`, `price`, and `quantity` fields from the `products` table sorted by `price` in _ascending_ order:

```sql
SELECT name, price, quantity FROM products
    ORDER BY price;
```

This query returns the `name`, `price`, and `quantity` of the products ordered by the quantity in _descending_ order:

```sql
SELECT name, price, quantity FROM products
    ORDER BY quantity DESC;
```