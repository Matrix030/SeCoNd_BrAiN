A `do while` loop in C is a control flow statement that allows code to be executed repeatedly based on a given boolean condition.

Unlike the `while` loop, the `do while` loop checks the condition after executing the loop body, so the loop body is **always** executed at least once.

## Syntax

```c
do {
    // Loop Body
} while (condition);
```

## Parts of a `do while` Loop

1. **Loop Body**
    - The block of code that is executed _before_ checking the condition, and then repeatedly as long as the condition is true.
2. **Condition**:
    - Checked _after_ each iteration.
    - If `true`, execute the body again.
    - If `false`, terminate the loop

## Examples

```c
#include <stdio.h>

int main() {
    int i = 0;
    do {
        printf("i = %d\n", i);
        i++;
    } while (i < 5);
    return 0;
}
// Prints:
// i = 0
// i = 1
// i = 2
// i = 3
// i = 4
```

```c
#include <stdio.h>

int main() {
    int i = 100;
    do {
        printf("i = %d\n", i);
        i++;
    } while (i < 5);
    return 0;
}
// Prints:
// i = 100
```

## Key Points

The `do while` loop guarantees that the loop body is executed at least once, even if the condition is false initially.

The most common scenario you will see a do-while loop used is in [C macros](https://gcc.gnu.org/onlinedocs/cpp/Macros.html) - they let you define a block of code and execute it exactly once in a way that is safe across different compilers, and ensures that the variables created/referenced within the macro do not leak to the surrounding environment.

If you end up looking at any source code for macros, you will probably see a few do-while loops. For example, here's a simplified version from our `munit` testing library we're using:

```c
#define munit_assert_type_full(T, fmt, a, op, b, msg)                          \
  do {                                                                         \
    T munit_tmp_a_ = (a);                                                      \
    T munit_tmp_b_ = (b);                                                      \
    if (!(munit_tmp_a_ op munit_tmp_b_)) {                                     \
      munit_errorf("assertion failed: %s %s %s (" prefix "%" fmt suffix        \
                   " %s " "%" fmt "): %s",                                     \
                   #a, #op, #b, munit_tmp_a_, #op, munit_tmp_b_, msg);         \
    }                                                                          \
  } while (0)
```

It creates a do-while loop, creates a few new variables and then checks that the assertion is valid. If it is not, then it errors and formats a (complicated) error message (If this code doesn't make any sense, that's fine too! I just wanted to show you where they most often occur).

There is no semi-colon after `while(0)` in the loop above. This lets the macro be used in a block of code without causing syntax errors.