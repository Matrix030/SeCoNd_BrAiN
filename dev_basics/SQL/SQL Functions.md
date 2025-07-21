SQL is a programming language and like nearly all programming languages, it supports functions. We can use functions and aliases to _calculate_ new columns in a query. This is similar to how you might use formulas in Excel.

A calculated column is a new column that doesn't exist in the original table but is created on the fly when you run a query.

## IIF Function

In SQLite, the `IIF` function works like a [ternary](https://book.pythontips.com/en/latest/ternary_operators.html). For example,

```sql
IIF(carA > carB, 'Car a is bigger', 'Car b is bigger')
```

If `carA` is greater than `carB`, this statement evaluates to the string `'Car a is bigger'`. Otherwise, it evaluates to `'Car b is bigger'`.

Here's how we can use `IIF()` and a `directive` alias to add a new calculated column to our result set:

```sql
SELECT quantity,
    IIF(quantity < 10, 'Order more', 'In Stock') AS directive
    FROM products;
```