Sometimes you need your generic function to know _something_ about the types it operates on. The example we used in the first exercise didn't need to know _anything_ about the types in the slice, so we used the built-in `any` constraint:

```go
func splitAnySlice[T any](s []T) ([]T, []T) {
    mid := len(s)/2
    return s[:mid], s[mid:]
}
```

Constraints are just interfaces that allow us to write generics that only operate within the constraints of a given interface type. In the example above, the `any` constraint is the same as the empty interface because it means the type in question can be _anything_.

## Creating a Custom Constraint

Let's take a look at the example of a `concat` function. It takes a slice of values and concatenates the values into a string. This should work with _any type that can represent itself as a string_, even if it's not a string under the hood. For example, a `user` struct can have a `.String()` that returns a string with the user's name and age.

```go
type stringer interface {
    String() string
}

func concat[T stringer](vals []T) string {
    result := ""
    for _, val := range vals {
        // this is where the .String() method
        // is used. That's why we need a more specific
        // constraint instead of the any constraint
        result += val.String()
    }
    return result
}
```