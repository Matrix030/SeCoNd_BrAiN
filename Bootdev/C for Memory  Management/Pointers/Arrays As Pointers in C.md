In C, arrays and pointers are closely related. An array name acts as a pointer to the first element of the array. That means array indexing and pointer arithmetic can be used interchangeably to access array elements. Let's go through this step-by-step to understand how this works.

## Step-by-Step Walkthrough

1. **Array Declaration**:

```c
int numbers[5] = {1, 2, 3, 4, 5};
```

Here, `numbers` is an array of 5 integers.

2. **Array as Pointer**:

The name `numbers` acts as a pointer to the first element of the array.

```c
int *numbers_ptr = numbers;
```

`numbers_ptr` is a pointer to the same place as `numbers`.

3. **Accessing Elements via Indexing**:

```c
// Access the third element (index 2)
int value = numbers[2];
```

Which is the same as:

```c
int value = *(numbers + 2);
```

Here, `numbers + 2` computes the address of the third element, and `*` dereferences it to get the value.

4. **Pointer Arithmetic**:

When you add an integer to a pointer, the resulting pointer is offset by that integer times the size of the data type.

```c
int *p = numbers + 2;  // p points to the third element
int value = *p;        // value is 3
```

## Diagram Explanation

Let's assume `numbers` is stored starting at memory address `0x1000`. An integer is typically 4 bytes in C. Here's how the array elements are laid out in memory:

| Address | Element    | Value |
| ------- | ---------- | ----- |
| 0x1000  | numbers[0] | 1     |
| 0x1004  | numbers[1] | 2     |
| 0x1008  | numbers[2] | 3     |
| 0x100C  | numbers[3] | 4     |
| 0x1010  | numbers[4] | 5     |

## Accessing Elements Using Pointers

- `numbers + 0` or `&numbers[0]` points to `0x1000`
- `numbers + 1` or `&numbers[1]` points to `0x1004`
- `numbers + 2` or `&numbers[2]` points to `0x1008`
- `numbers + 3` or `&numbers[3]` points to `0x100C`
- `numbers + 4` or `&numbers[4]` points to `0x1010`

## Example Code

```c
#include <stdio.h>

int main() {
  int numbers[5] = {1, 2, 3, 4, 5};

  // Accessing elements using array indexing
  printf("numbers[2] = %d\n", numbers[2]);  // Output: 3

  // Accessing elements using pointers
  printf("*(numbers + 2) = %d\n", *(numbers + 2));  // Output: 3

  // Pointer arithmetic
  int *ptr = numbers;
  printf("Pointer ptr points to numbers[0]: %d\n", *ptr);  // Output: 1
  ptr += 2;
  printf("Pointer ptr points to numbers[2]: %d\n", *ptr);  // Output: 3

  return 0;
}
```