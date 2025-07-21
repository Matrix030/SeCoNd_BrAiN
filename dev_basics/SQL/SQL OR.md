As you've probably guessed, if the logical `AND` operator is supported, the `OR` operator is probably supported as well.

```sql
SELECT product_name, quantity, shipment_status
    FROM products
    WHERE shipment_status = 'out of stock'
    OR quantity BETWEEN 10 and 100;
```

This query retrieves records where _either_ the `shipment_status` condition _OR_ the `quantity` condition are met.

## Order of Operations

You can group logical operations with parentheses to specify the [order of operations](https://www.mathsisfun.com/operation-order-pemdas.html).

```sql
(this AND that) OR the_other
```