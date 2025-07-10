When multiple arguments are of the same type, and are next to each other in the function signature, the type only needs to be declared after the last argument.

Here are some examples:

```go
func addToDatabase(hp, damage int) {
  // ...
}
```

```go
func addToDatabase(hp, damage int, name string) {
  // ?
}
```

```go
func addToDatabase(hp, damage int, name string, level int) {
  // ?
}
```