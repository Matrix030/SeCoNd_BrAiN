By now, you're probably tired of typing `struct Coordinate` over and over again, and you're wondering "How can I make my struct types easier to write, like `int`?"

Good news! C can do this with the [`typedef` keyword](https://en.cppreference.com/w/c/language/typedef).

```c
struct Pastry {
    char *name;
    float weight;
};
```

This can also be written as:

```c
typedef struct Pastry {
    char *name;
    float weight;
} pastry_t;
```

Now, you can use `pastry_t` wherever before you would have used `struct Pastry`.

The `_t` at the end is a common convention to indicate a type.

In fact, you can optionally skip giving the struct a name:

```c
typedef struct {
    char *name;
    float weight;
} pastry_t;

pastry_t muffin = {"Muffin", 0.3};
```

In this case you'd only be able to refer to the type as `pastry_t`. In general, I _do_ give the struct an actual name (e.g. `Pastry`), but I only use the `typedef`'d type. _We'll be using this convention in this course._