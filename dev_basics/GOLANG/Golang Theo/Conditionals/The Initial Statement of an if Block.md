An `if` conditional can have an "initial" statement. The variable(s) created in the initial statement are _only_ defined within the scope of the `if` body.

```go
if INITIAL_STATEMENT; CONDITION {
}
```
## Why Would I Use This?

It has two valuable purposes:

1. It's a bit shorter
2. It limits the scope of the initialized variable(s) to the `if` block

For example, instead of writing:

```go
length := getLength(email)
if length < 1 {
    fmt.Println("Email is invalid")
}
```

We can do:

```go
if length := getLength(email); length < 1 {
    fmt.Println("Email is invalid")
}
```

In the example above, `length` isn't available in the parent scope, which is nice because we don't need it there - we won't accidentally use it elsewhere in the function.