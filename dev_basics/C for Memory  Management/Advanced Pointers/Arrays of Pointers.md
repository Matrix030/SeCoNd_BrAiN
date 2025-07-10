Making an array of integers on the heap is pretty simple:

```c
int *int_array = malloc(sizeof(int) * 3);
int_array[0] = 1;
int_array[1] = 2;
int_array[2] = 3;
```

But we can also make an arrFay of pointers! It's quite common to do this in C, especially considering that strings are just pointers to `char`s:

```c
char **string_array = malloc(sizeof(char *) * 3);
string_array[0] = "foo";
string_array[1] = "bar";
string_array[2] = "baz";
```