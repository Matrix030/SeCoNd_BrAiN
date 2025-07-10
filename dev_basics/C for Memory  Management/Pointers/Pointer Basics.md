Remember, pointers are just an address (read: value) that tells the computer where to look for _other_ values. Just like how the address to your house is not actually your house, but points you to where your house **is**.

## Syntax Review

Declare a pointer to an integer:

```c
int *pointer_to_something; // declares `pointer_to_something` as a pointer to an int
```

Get the address of a variable:

```c
int meaning_of_life = 42;
int *pointer_to_mol = &meaning_of_life;
// pointer_to_mol now holds the address of meaning_of_life
```

## New: Dereferencing Pointers

Oftentimes we have a pointer, but we want to get access to the data that it points to. Not the address itself, but the value stored at _that_ address.

We can use an asterisk (`*`) to do it. The `*` operator dereferences a pointer.

```c
int meaning_of_life = 42;
int *pointer_to_mol = &meaning_of_life;
int value_at_pointer = *pointer_to_mol;
// value_at_pointer = 42
```

_It can be a touch confusing, but remember that the asterisk symbol is used for two different things:_

1. Declaring a pointer type: `int *pointer_to_thing;`
2. Dereferencing a pointer value: `int value = *pointer_to_thing;` (retrieving the value) or `*pointer_to_thing = 20;` (modifying the value)