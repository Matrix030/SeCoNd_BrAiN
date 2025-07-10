A `for` loop in C is a control flow statement for repeated execution of a block of code. Very similar to Python, but with a different syntax.

The syntax of a `for` loop in C consists of three main parts:

1. Initialization
2. Condition
3. Final-expression.

There is no "for each" (iterables) in C. For example, there is no way to do:

```python
for car in cars:
    print(car)
```

Instead, we have to iterate over the numbers of indices in a list, and then we can access the item using the index.

## Syntax

```c
for (initialization; condition; final-expression) {
    // Loop Body
}
```

## Parts of a `for` Loop

1. **Initialization**
    - Executed only once at the beginning of the loop.
    - Is typically used to initialize the loop counter: `int i = 0;` for example
2. **Condition**
    - Checked before each iteration.
    - If `true`, execute the body. If `false`, terminate the loop
    - Often checks to ensure `i` is less than some value: `i < 5;` for example
3. **Final-expression**
    - Executed after each iteration of the loop body.
    - Can be used to update the loop counter or run any other code: `i++` for example
4. **Loop Body**
    - The block of code that is executed while the condition is true.

## Example: Basic Loop

```c
#include <stdio.h>

int main() {
  for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
  }
  return 0;
}

// Prints:
// 0
// 1
// 2
// 3
// 4
```