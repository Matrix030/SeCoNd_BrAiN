We've already discussed `void`, which essentially means "nothing" in C. It's used in a few different contexts:

- `void update_soldier(soldier_t *s)`: means the function returns nothing
- `soldier_t new_soldier(void)`: means the function takes no arguments.

And, because C likes re-using ideas but with slightly different meanings (the genius of the design can't be understood by us mere mortals) `void` also has another use!

A `void *` "void pointer" tells the compiler that this pointer could point to **anything**. This is why void pointers are also known as a "generic pointer". Since void pointers do not have a specific data type, they cannot be directly dereferenced or used in pointer arithmetic without casting them to another pointer type first.

## Casting to Void Pointers

Casting to and from void pointers in C is unique because void pointers are type-agnostic. When casting a specific type pointer to a void pointer, no type information is retained, allowing the void pointer to point to any data type. However, you **must** cast a void pointer back to its original type before dereferencing it, as direct dereferencing is not possible.

```c
int number = 42;
void *generic_ptr = &number;

// This doesn't work
printf("Value of number: %d\n", *generic_ptr);

// This works: Cast to appropriate type before dereferencing
printf("Value of number: %d\n", *(int*)generic_ptr);
```

A common pattern is to store generic data in one variable, and the type of that data in another variable. This is useful when you need to pass data around without knowing its type at compile time.

```c
typedef enum DATA_TYPE {
  INT,
  FLOAT
} data_type_t;

void printValue(void *ptr, data_type_t type) {
  if (type == INT) {
    printf("Value: %d\n", *(int*)ptr);
  } else if (type == FLOAT) {
    printf("Value: %f\n", *(float*)ptr);
  }
}

int number = 42;
printValue(&number, INT);

float decimal = 3.14;
printValue(&decimal, FLOAT);
```