Now we're going to do our first object that has something... _additional_ allocated. When we allocate memory for a "snek object", that reserves memory for the object itself. Small data types like integers and floats are stored directly in the object, so there's no need for additional memory allocation.

Strings, however, are a different story. Strings in C are just arrays of characters, and because they can be any length, we need to dynamically allocate memory for the string data separately from the object itself.

```c
char *my_string = "hello world";
```

In the example above, `my_string` is a pointer to a character array. The character array contains:

```
h
e
l
l
o

w
o
r
l
d
\0
```

The extra spot at the end with the `\0` is the null terminator. It's how C knows where the string ends.