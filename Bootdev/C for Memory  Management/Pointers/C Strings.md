Since the beginning of the course we've been doing these shenanigans to be able to print strings:

```c
char *msg = "ssh terminal.shop for the best coffee";
```

I told you not to worry about the weird `char *` syntax, but now that we understand a bit about pointers, let's dive into it. In the example above, `msg` is a pointer to the first character of the string `"ssh terminal.shop for the best coffee"`, which is a [C string](https://en.wikipedia.org/wiki/C_string_handling). C strings are:

- How we represent text in C programs
- Any number of characters (`char`s) terminated by a null character (`'\0'`).
- A pointer to the first element of a character array.

It's important to understand that most string manipulation in C is done using pointers to move around the array and the null terminator is critical for determining the end of the string. In the example above, the string `"ssh terminal.shop for the best coffee"` is stored in memory as an array of characters, and the null terminator `'\0'` is automatically added at the end.

## C Strings Are Simple

- Unlike other programming languages, C strings do _not_ store their length.
- The length of a C string is determined by the position of the null terminator (`'\0'`).
- Functions like [`strlen`](https://en.cppreference.com/w/c/string/byte/strlen) calculate the length of a string by iterating through the characters until the null terminator is encountered.
- This lack of length storage requires careful management to avoid issues such as buffer overflows and off-by-one errors during string operations.

## Pointers vs. Arrays

You can declare strings in C using either arrays or pointers:

```c
char str1[] = "Hi";
char *str2 = "Snek";
printf("%s %s\n", str1, str2);
// Output: Hi Snek
```

The output is the same. Let's break down the memory of this example:

```c
// notice we aren't using all 50 characters
char first[50] = "Snek";
char *second = "lang!";
strcat(first, second);
printf("Hello, %s\n", first);
// Output: Hello, Sneklang!
```

The [`strcat`](https://en.cppreference.com/w/c/string/byte/strcat) function appends its second argument to the first argument. In this case, it appends `"lang!"` to `"Snek"`, resulting in the output `Hello, Sneklang!`.

Here's what `first` might look like in memory:

|'S'|'n'|'e'|'k'|'\0'|????|...|????|
|---|---|---|---|---|---|---|---|
|0x3000|0x3001|0x3002|0x3003|0x3004|0x3005|...|0x3031|

_NOTE! There is a bunch of garbage memory after the end of the string_.

Here's what `second` might look like in memory:

|'l'|'a'|'n'|'g'|'!'|'\0'|
|---|---|---|---|---|---|
|0x4000|0x4001|0x4002|0x4003|0x4004|0x4005|

And `first` after `strcat`:

|'S'|'n'|'e'|'k'|'l'|'a'|'n'|'g'|'!'|'\0'|????|...|????|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0x3000|0x3001|0x3002|0x3003|0x3004|0x3005|0x3006|0x3007|0x3008|0x3009|0x300A|...|0x3031|

The `strcat` function appends the string `"lang!"` to the end of the string `"Snek"`, but smartly uses the null terminator to know where to start appending. It doesn't know the length of the string, but it knows where it ends.

Example:
```c
#include "exercise.h"
#include "stdio.h"
void concat_strings(char *str1, const char *str2) {
    while(*str1){
      printf("%c", *str1);
      str1++;
    }
    while(*str2){
      *str1 = *str2;
      str1++;
      str2++;
    }

  *str1 = '\0';
    
}

```
