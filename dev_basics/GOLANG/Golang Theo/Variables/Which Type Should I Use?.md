With so many types for what is essentially just a number, developers coming from languages that only have one kind of `Number` type (like JavaScript) may find the choices daunting.

## Prefer “Default” Types

A problem arises when we have a `uint16`, and the function we are trying to pass it into takes an `int`. We're forced to write code riddled with type conversions like:

```go
var myAge uint16 = 25
myAgeInt := int(myAge)
```

This style of code can be slow and annoying to read. When Go developers stray from the “default” type for any given type family, the code can get messy quickly. Unless you have a good _performance related_ reason, you'll typically just want to use the "default" types:

- `bool`
- `string`
- `int`
- `uint`
- `byte`
- `rune`
- `float64`
- `complex128`

## When Should I Use a More Specific Type?

When you're super concerned about performance and memory usage.

That’s about it. The only reason to deviate from the defaults is to squeeze out every last bit of performance when you are writing an application that is resource-constrained. (Or, in the special case of `uint64`, you need an absurd range of unsigned integers).

You can [read more on this subject here](https://blog.boot.dev/golang/default-native-types-golang/) if you'd like, but it's not required.