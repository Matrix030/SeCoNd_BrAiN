Remember, interfaces are collections of method signatures. A type "implements" an interface if it has all of the methods of the given interface defined on it.

```go
type shape interface {
  area() float64
}
```

If a type in your code implements an `area` method, with the same signature (e.g. accepts nothing and returns a `float64`), then that object is said to _implement_ the `shape`interface.

```go
type circle struct{
	radius float64
}

func (c circle) area() float64 {
  return 3.14 * c.radius * c.radius
}
```

This is _different from most other languages_, where you have to _explicitly_ assign an interface type to an object, like with Java:

```java
class Circle implements Shape
```