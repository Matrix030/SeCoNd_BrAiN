An "aggregation" is a _single_ value that's derived by combining _several_ other values. We performed an aggregation earlier when we used the `COUNT` statement to count the number of records in a table.

## Why Aggregations?

Data stored in a database should generally be stored [raw](https://wagslane.dev/posts/keep-your-data-raw-at-rest/). When we need to calculate some additional data from the raw data, we can use an _aggregation_.

Take the following `COUNT` aggregation as an example:

```sql
SELECT COUNT(*)
FROM products
WHERE quantity = 0;
```

This query returns the number of products that have a `quantity` of `0`. We _could_ store a count of the products in a separate database table, and increment/decrement it whenever we make changes to the `products` table - but that would be _redundant_.

It's _much simpler_ to store the products in a single place (we call this a [single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth)) and run an aggregation when we need to derive additional information from the raw data.