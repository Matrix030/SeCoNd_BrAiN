Methods with pointer receivers don't require that a pointer is used to call the method. The pointer will automatically be derived from the value.

```go
type circle struct {
	x int
	y int
    radius int
}

func (c *circle) grow() {
    c.radius *= 2
}

func main() {
    c := circle{
        x: 1,
        y: 2,
        radius: 4,
    }

    // notice c is not a pointer in the calling function
    // but the method still gains access to a pointer to c
    c.grow()
    fmt.Println(c.radius)
    // prints 8
}
```

## Simpler Explaination:

In Go, if a method is defined using a pointer receiver (like `func (c *circle) grow()`), you **don't need to use a pointer** when calling that method. Go will **automatically convert the value to a pointer** for you.

### Example Breakdown:

```go
func (c *circle) grow() {
    c.radius *= 2
}
```

This method wants a pointer to a `circle`, so it can **change** the original object (like doubling the radius).

But in `main()`:

```go
c := circle{...}
c.grow()
```

Even though `c` is **not a pointer**, Go automatically treats it like `&c`, so the method works as expected and changes the original `c`.

### In short:

You can call a pointer method on a value, and Go takes care of turning it into a pointer behind the scenes.