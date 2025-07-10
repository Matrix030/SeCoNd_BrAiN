A `while` loop in C is a control flow statement that allows code to be executed repeatedly based on a given boolean (`true`/`false`) condition. The loop continues to execute as long as the condition remains true.

## Syntax

```c
while (condition) {
    // Loop Body
}
```

## Parts of a `while` Loop

1. **Condition**
    - Checked before each iteration.
    - If `true`, execute the body. If `false`, terminate the loop
2. **Loop Body**
    - The block of code that is executed while `condition` is true.

## Example: Basic Loop

```c
#include <stdio.h>

int main() {
    int i = 0;
    while (i < 5) {
        printf("%d\n", i);
        i++;
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

## Key Points

- The condition is evaluated _before_ the execution of the loop body.
- If the condition is `false` initially, the loop body will never even start.
- If the condition never becomes `false`, you will get an infinite loop.