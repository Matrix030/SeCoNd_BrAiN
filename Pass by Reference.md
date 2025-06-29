Functions in Go generally pass variables by value, meaning that functions receive a copy of most non-composite types:

```go
func increment(x int) {
    x++
    fmt.Println(x)
    // 6
}


func main() {
    x := 5
    increment(x)
    fmt.Println(x)
    // 5
}
```

The `main` function still prints `5` because the `increment` function received a _copy_ of `x`.

One of the most common use cases for pointers in Go is to pass variables by reference, meaning that the function receives the _address_ of the original variable, not a copy of the value. This allows the function to **update the original variable's value**.

```go
func increment(x *int) {
    *x++
    fmt.Println(*x)
    // 6
}

func main() {
    x := 5
    increment(&x)
    fmt.Println(x)
    // 6
}
```

## Fields of Pointers

When your function receives a pointer to a struct, you might try to access a field like this and encounter an error:

```go
msgTotal := *analytics.MessagesTotal
```

Instead, access it – like you'd normally do — using a [selector expression](https://go.dev/ref/spec#Selectors).

```go
msgTotal := analytics.MessagesTotal
```

This approach is the recommended, simplest way to access struct fields in Go, and is shorthand for:

```go
(*analytics).MessagesTotal
```