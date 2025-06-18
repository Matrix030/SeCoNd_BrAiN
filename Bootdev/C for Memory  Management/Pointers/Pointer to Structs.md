As you know, when you have a struct, you can access the fields with the dot (`.`) operator:

```c
coordinate_t point = {10, 20, 30};
printf("X: %d\n", point.x);
```

However, when you're working with a _pointer to a struct_, you need to use the arrow (`->`) operator:

```c
coordinate_t point = {10, 20, 30};
coordinate_t *ptrToPoint = &point;
printf("X: %d\n", ptrToPoint->x);
```

It effectively dereferences the pointer and accesses the field in one step. To be fair, you can also use the dereference and dot operator (`*` and `.`) to achieve the same result (it's just more verbose and less common):

```c
coordinate_t point = {10, 20, 30};
coordinate_t *ptrToPoint = &point;
printf("X: %d\n", (*ptrToPoint).x);
```

## Order of Operations

The `.` operator has a higher precedence than the `*` operator, so parentheses are _necessary_ when using `*` to dereference a pointer before accessing a member... which is another reason why the arrow operator is so much more common.