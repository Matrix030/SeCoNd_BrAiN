A [type switch](https://go.dev/tour/methods/16) makes it easy to do several type assertions in a series.

A type switch is similar to a regular switch statement, but the cases specify _types_ instead of _values_.

```go
func printNumericValue(num interface{}) {
	switch v := num.(type) {
	case int:
		fmt.Printf("%T\n", v)
	case string:
		fmt.Printf("%T\n", v)
	default:
		fmt.Printf("%T\n", v)
	}
}

func main() {
	printNumericValue(1)
	// prints "int"

	printNumericValue("1")
	// prints "string"

	printNumericValue(struct{}{})
	// prints "struct {}"
}
```

`fmt.Printf("%T\n", v)` prints the _type_ of a variable.