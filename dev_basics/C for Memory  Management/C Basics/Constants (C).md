So a variable's _value_ can change:

```c
int main() {
    int x = 5;
    x = 10; // this is ok
}
```

But what if we want to create a value that _can't_ change? We can use the [`const` type qualifier](https://en.cppreference.com/w/c/language/const).

```c
int main() {
    const int x = 5;
    x = 10; // error
}
```