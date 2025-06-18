If you're used to [Lists in Python](https://docs.python.org/3/tutorial/datastructures.html), [Arrays in C](https://en.cppreference.com/w/c/language/array) are _similar_, but a bit lower level.

An array is a _fixed-size_, ordered collection of elements. Like Python lists, they are indexed by integers, starting at zero. Unlike Python lists, they can only hold elements of the same type. They are stored in contiguous memory, like structs.

## Integer Array

```c
int numbers[5] = {1, 2, 3, 4, 5};
```

### Iterating Over an Array

In C, there is no `for x in list:` syntax. Instead, you must iterate over them using a `for` loop with an index (or some other conditional loop)

```c
#include <stdio.h>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    // Iterate and print each element
    for (int i = 0; i < 5; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");

    return 0;
}
```

Output:

```
1 2 3 4 5
```

### Updating Values in an Array

The syntax for updating values in an array is the same as how you access them:

`arr[index] = value`

Using our `numbers` example:

```c
#include <stdio.h>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};

    // Update some values
    numbers[1] = 20;
    numbers[3] = 40;

    // Print updated array
    for (int i = 0; i < 5; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");

    return 0;
}
```

Output:

```
1 20 3 40 5
```