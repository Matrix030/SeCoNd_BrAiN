Integers, [uints](https://www.cs.utah.edu/~germain/PPS/Topics/unsigned_integer.html#:~:text=Unsigned%20Integers,negative%20\(zero%20or%20positive\).), [floats](https://techterms.com/definition/floatingpoint), and [complex](https://www.cloudhadoop.com/2018/12/golang-tutorials-complex-types-numbers.html#:~:text=Golang%20Complex%20Type%20Numbers,complex%20number%20is%2012.8i.) numbers all have type sizes.

## Whole Numbers (No Decimal)

```go
int  int8  int16  int32  int64
```

## Positive Whole Numbers (No Decimal)

_"uint" stands for "unsigned integer"._

```go
uint uint8 uint16 uint32 uint64 uintptr
```

## Signed Decimal Numbers

```go
float32 float64
```

## Complex Numbers (a Complex Number Has a Real and Imaginary Part)

```go
complex64 complex128
```

## What's the Deal With the Sizes?

The size (8, 16, 32, 64, 128, etc) represents how many [bits](https://en.wikipedia.org/wiki/Bit) in memory will be used to store the variable. The "default" `int` and `uint` types refer to their respective 32 or 64-bit sizes depending on the environment of the user.

The "standard" sizes that should be used unless you have a specific performance need (e.g. using less memory) are:

- `int`
- `uint`
- `float64`
- `complex128`

## Converting Between Types

Some types can be easily converted like this:

```go
temperatureFloat := 88.26
temperatureInt := int64(temperatureFloat)
```

Casting a float to an integer in this way [truncates](https://techterms.com/definition/truncate) the floating point portion.