Like JavaScript, C has a ternary operator:

```c
int a = 5;
int b = 10;
int max = a > b ? a : b;
printf("max: %d\n", max);
// max: 10
```

Let's break down the syntax:

```c
a > b ? a : b
```

The entire line is a single expression that evaluates to one value. Here's how it works:

- `a > b` is the condition
- `a` is the final value if the condition is true
- `b` is the final value if the condition is false
- The entire expression (`a > b ? a : b`) evaluates to either `a` or `b`, which is then assigned to `max` in our example.

_Ternaries are a way to write a simple if/else statement in one line._