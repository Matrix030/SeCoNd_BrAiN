The size of an array depends on both the number of elements and the size of each element. An array is a contiguous block of memory where each element has a specific type, and therefore, a specific size.

In C, pointers are always the same size because they just represent memory addresses. The size of a pointer is determined by the architecture of the system (e.g., 32-bit or 64-bit). A pointer's size doesn't depend on the type of data it points to; it just holds the address of a memory location.

## Pointer Example

```c
int *intPtr;
char *charPtr;
double *doublePtr;
printf("Size of int pointer: %zu bytes\n", sizeof(intPtr));
printf("Size of char pointer: %zu bytes\n", sizeof(charPtr));
printf("Size of double pointer: %zu bytes\n", sizeof(doublePtr));
// Size of int pointer: 4 bytes
// Size of char pointer: 4 bytes
// Size of double pointer: 4 bytes
```

They're all the same [size](https://port70.net/~nsz/c/c11/n1570.html#6.5.3.4), because they're all just 32-bit memory addresses: it doesn't matter how much memory the value at that address takes up.

## Array Example

```c
int intArray[10];
char charArray[10];
double doubleArray[10];
printf("Size of int array: %zu bytes\n", sizeof(intArray));
printf("Size of char array: %zu bytes\n", sizeof(charArray));
printf("Size of double array: %zu bytes\n", sizeof(doubleArray));
// Size of int array: 40 bytes
// Size of char array: 10 bytes
// Size of double array: 80 bytes
```

Now the sizes are different because the array type keeps track of the size of each element and the number of elements. Although an array is a pointer to the first element, it's not _just_ a pointer: it's a block of memory that holds all the elements.

Boot.dev runs C in the browser using [WASM](https://webassembly.org/), which is typically a 32-bit system. If you run this code on a 64-bit system, the size of the pointers will be 8 bytes.