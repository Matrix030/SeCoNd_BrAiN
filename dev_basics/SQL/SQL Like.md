Sometimes we don't have the luxury of knowing _exactly_ what it is we need to query. Have you ever wanted to look up a song or a video but you only remember _part_ of the name? SQL provides us an option for when we're in situations `LIKE` this.

The `LIKE` keyword allows for the use of the `%` and `_` wildcard operators. Let's focus on `%` first.

## % Operator

The `%` operator will match zero or more characters. We can use this operator within our query string to find more than just exact matches depending on where we place it.

## Product Starts With “banana”:

```sql
SELECT * FROM products
WHERE product_name LIKE 'banana%';
```

## Product Ends With “banana”:

```sql
SELECT * FROM products
WHERE product_name LIKE '%banana';
```

## Product Contains “banana”:

```sql
SELECT * FROM products
WHERE product_name LIKE '%banana%';
```