Most of the time we don't need to think about the underlying array of a slice. We can create a new slice using the [make](https://pkg.go.dev/builtin#make) function:

```go
// func make([]T, len, cap) []T
mySlice := make([]int, 5, 10)

// the capacity argument is usually omitted and defaults to the length
mySlice := make([]int, 5)
```

Slices created with `make` will be filled with the zero value of the type.

If we want to create a slice with a specific set of values, we can use a slice literal:

```go
mySlice := []string{"I", "love", "go"}
```

Notice the square brackets _do not_ have a `3` in them. If they did, you'd have an _array_ instead of a slice.

## Length

The length of a slice is simply the number of elements it contains. It is accessed using the built-in `len()` function:

```go
mySlice := []string{"I", "love", "go"}
fmt.Println(len(mySlice)) // 3
```

## Capacity

The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice. It is accessed using the built-in `cap()` function:

```go
mySlice := []string{"I", "love", "go"}
fmt.Println(cap(mySlice)) // 3
```

Generally speaking, unless you're hyper-optimizing the memory usage of your program, you don't need to worry about the capacity of a slice because it will automatically grow as needed.

## Indexing

A programming concept you should already be familiar with is [array indexing](https://go.dev/blog/slices-intro#arrays). You can access or assign a single element of an array or slice by using its [index](https://en.wikipedia.org/wiki/Zero-based_numbering).

```go
mySlice := []string{"I", "love", "go"}
fmt.Println(mySlice[2]) // go

mySlice[0] = "you"
fmt.Println(mySlice) // [you love go]
```