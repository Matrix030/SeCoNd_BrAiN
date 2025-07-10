As we talked about, variables cannot change _types_:

```c
int main() {
    int x = 5;
    float x = 3.14; // error
}
```

However, a variable's _value_ can change:

```c
int main() {
    int x = 5;
    x = 10; // this is ok
    x = 15; // still ok
}
```