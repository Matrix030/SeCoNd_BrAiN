You've probably heard of pointers. You may have also seen jokes about how they are impossible to learn... Well, that's _wrong_.

In fact, now that you understand how memory is laid out in an array, a lot of the mystery behind pointers should be gone. Put simply: **a pointer is just a variable that stores a memory address**. It's called a pointer, because it "points" to the address of a variable, which stores the actual data held in that variable.
## Syntax

A pointer is declared with an asterisk (`*`) after the type. For example, `int *`.

```c
int age = 37;
int *pointer_to_age = &age;
```

Remember, to get the address of a variable so that we can store it _in_ a pointer variable, we can use the address-of operator (`&`).