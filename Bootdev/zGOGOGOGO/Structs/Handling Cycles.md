We built a simple reference counting garbage collector. It can handle:

- Simple types, like `INT` and `FLOAT`.
- Dynamically allocated types, like `STRING`.
- Static Container types, like `VECTOR3`.
- Dynamic Container types, like `ARRAY`.

However, there's one little problem with our implementation. Take a look at this code:

```c
snek_object_t *first = new_snek_array(1);
snek_object_t *second = new_snek_array(1);
// refcounts: first = 1, second = 1
snek_array_set(first, 0, second);
// refcounts: first = 1, second = 2
refcount_dec(first);
// refcounts: first = 0, second = 1
refcount_dec(second);
// refcounts: first = 0, second = 0
// all free!
```

We create a `first` array, and shove the `second` array inside of it. Everything here works as expected. The trouble arises when we introduce a cycle: for example, `first` contains `second` but `second` also contains `first`...