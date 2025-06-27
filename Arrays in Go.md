Arrays are fixed-size groups of variables of the same type. For example, `[4]string` is an array of `4` values of type `string`.

To declare an array of 10 integers:

```go
var myInts [10]int
```

or to declare an initialized literal:

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
```