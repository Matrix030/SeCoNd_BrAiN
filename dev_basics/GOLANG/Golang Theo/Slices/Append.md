The built-in append function is used to dynamically add elements to a slice:

```go
func append(slice []Type, elems ...Type) []Type
```

If the underlying array is not large enough, `append()` will create a new underlying array and point the returned slice to it.

Notice that `append()` is variadic, the following are all valid:

```go
slice = append(slice, oneThing)
slice = append(slice, firstThing, secondThing)
slice = append(slice, anotherSlice...)
```