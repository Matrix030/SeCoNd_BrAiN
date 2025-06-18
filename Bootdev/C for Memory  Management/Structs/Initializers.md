So now you're probably wondering: "Hey TJ, so... how do we actually _make an instance of_ a struct"? You may have noticed in the previous lesson all we did was _define the struct type_.

Unfortunately, there are a few different ways to [initialize a struct](https://en.cppreference.com/w/c/language/struct_initialization), I'll give you an example of each using this struct:

```c
struct City {
  char *name;
  int lat;
  int lon;
};
```

## Zero Initializer

```c
int main() {
  struct City c = {0};
}
```

This sets all the fields to `0` values.

## Positional Initializer

```c
int main() {
  struct City c = {"San Francisco", 37, -122};
}
```

## Designated Initializer

**This is my (generally) preferred way to initialize a struct.** Why?

- It's easier to read (has the field names)
- If the fields change, you don't have to worry about breaking the ordering

```c
int main() {
  struct City c = {
    .name = "San Francisco",
    .lat = 37,
    .lon = -122
  };
}
```

Remember, it's `.name` not `name`. If this trips you up, just remember it's `.name` and not `name` because that's how you access the field, e.g. `c.name`.

## Accessing Fields

Accessing a field in a struct is done using the `.` operator. For example:

```c
struct City c;
c.lat = 41; // Set the latitude
printf("Latitude: %d", c.lat); // Print the latitude
```

There's another way to do this for pointers that we'll get to later.