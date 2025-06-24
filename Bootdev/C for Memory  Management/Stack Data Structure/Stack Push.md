Ok, so let's actually store some data instead of just allocating memory for no practical purpose (a.k.a "haskell programming").

## Making Room

As you know, our stack has a `count` and a `capacity`... but what happens when the `count` is equal to the `capacity`? We need to make room for more data!

We'll take a simple approach: whenever we run out of capacity, we'll double it. That way we don't have to reallocate memory on every push. For example:

| Count | Capacity | Data                     |
| ----- | -------- | ------------------------ |
| 0     | 4        | [-, -, -, -]             |
| 1     | 4        | [1, -, -, -]             |
| 2     | 4        | [1, 2, -, -]             |
| 3     | 4        | [1, 2, 3, -]             |
| 4     | 4        | [1, 2, 3, 4]             |
| 5     | 8        | [1, 2, 3, 4, 5, -, -, -] |
