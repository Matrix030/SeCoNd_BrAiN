Remember how we can **not** return multiple values from a function in C? We can't do this:

```c
int, char * become_older(int age, char *name) {
  return age + 1, name;
}
```

However, we _can_ accomplish effectively the same thing by returning a `struct`:

```c
struct Human become_older(int age, char *name) {
  struct Human h = {.age = age, .name = name};
  h.age++;
  return h;
}
```