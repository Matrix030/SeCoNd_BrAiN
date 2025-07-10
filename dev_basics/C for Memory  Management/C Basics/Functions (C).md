In C, functions specify the types for their arguments and return value.

```c
float add(int x, int y) {
    return (float)(x + y);
}
```

- The first type, `float` is the return type.
- `add` is the name of the function.
- `int x, int y` are the arguments to the function, and their types are specified.
- `x + y` adds the two arguments together.
- `(float)` casts the result to a float.
    - We'll talk more about what [`cast`](https://en.wikipedia.org/wiki/Type_conversion) means later, and the rules for casting to and from certain types.
    - The simple version is that it instructs C to treat the result of `x + y` as a float.

Here's how you would call this function:

```c
int main() {
    float result = add(10, 5);
    printf("result: %f\n", result);
    // result: 15.000000
    return 0;
}
```

It's nice that C functions enforce returning the same type from all return statements, isn't it? In Python, it can be a pain to realize that a function returns different types depending on the path it took.