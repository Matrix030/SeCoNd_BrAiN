Forward declarations can also be used when two structs reference each other (a circular reference). For example, a `Person` has a `Computer` and a `Computer` has a `Person`:

```c
typedef struct Computer computer_t;
typedef struct Person person_t;

struct Person {
  char *name;
  computer_t *computer;
};

struct Computer {
  char *brand;
  person_t *owner;
};
```