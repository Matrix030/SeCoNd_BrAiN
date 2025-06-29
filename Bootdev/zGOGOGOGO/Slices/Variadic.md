Many functions, especially those in the standard library, can take an arbitrary number of _final_ arguments. This is accomplished by using the "..." syntax in the function signature.

A variadic function receives the variadic arguments as a slice.

```go
func concat(strs ...string) string {
    final := ""
    // strs is just a slice of strings
    for i := 0; i < len(strs); i++ {
        final += strs[i]
    }
    return final
}

func main() {
    final := concat("Hello ", "there ", "friend!")
    fmt.Println(final)
    // Output: Hello there friend!
}
```

The familiar [`fmt.Println()`](https://pkg.go.dev/fmt#Println) and [`fmt.Sprintf()`](https://pkg.go.dev/fmt#Sprintf) are variadic! `fmt.Println()` prints each element with space [delimiters](https://www.dictionary.com/browse/delimiter) and a newline at the end.

```go
func Println(a ...interface{}) (n int, err error)
```

## Spread Operator

The spread operator allows us to pass a slice _into_ a variadic function. The spread operator consists of three dots following the slice in the function call.

```go
func printStrings(strings ...string) {
	for i := 0; i < len(strings); i++ {
		fmt.Println(strings[i])
	}
}

func main() {
    names := []string{"bob", "sue", "alice"}
    printStrings(names...)
}
```