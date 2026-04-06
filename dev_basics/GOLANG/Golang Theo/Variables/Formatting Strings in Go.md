---
tags: [dev-basics, golang]
---
Go follows the [printf tradition](https://cplusplus.com/reference/cstdio/printf/) from the C language. In my opinion, string formatting/interpolation in Go is _less_ elegant than Python's f-strings, unfortunately.

- [fmt.Printf](https://pkg.go.dev/fmt#Printf) - Prints a formatted string to [standard out](https://stackoverflow.com/questions/3385201/confused-about-stdin-stdout-and-stderr).
- [fmt.Sprintf()](https://pkg.go.dev/fmt#Sprintf) - Returns the formatted string

These following "formatting verbs" work with the formatting functions above:

## Default Representation

The `%v` variant prints the Go syntax representation of a value, it's a nice default.

```go
s := fmt.Sprintf("I am %v years old", 10)
// I am 10 years old

s := fmt.Sprintf("I am %v years old", "way too many")
// I am way too many years old
```

If you want to print in a more specific way, you can use the following formatting verbs:

## String

```go
s := fmt.Sprintf("I am %s years old", "way too many")
// I am way too many years old
```

## Integer

```go
s := fmt.Sprintf("I am %d years old", 10)
// I am 10 years old
```

## Float

```go
s := fmt.Sprintf("I am %f years old", 10.523)
// I am 10.523000 years old

// The ".2" rounds the number to 2 decimal places
s := fmt.Sprintf("I am %.2f years old", 10.523)
// I am 10.52 years old
```

If you're interested in all the formatting options, you can look at the `fmt` package's [docs](https://pkg.go.dev/fmt#hdr-Printing).



# Easier way
### 💡 Basic Idea

In Go, you use special functions to insert values into strings, like this:

- `fmt.Printf(...)`: **prints** the result directly.
    
- `fmt.Sprintf(...)`: **returns** the string, so you can store or use it later.
    

---

### 🔧 Format Verbs (Placeholders)

Go uses **formatting verbs** (like `%v`, `%s`, `%d`, etc.) to say what kind of value you're inserting.

#### 🧩 The Most Common Verbs:

- `%v` → A general "default" way to print any value
    
- `%s` → For strings
    
- `%d` → For integers
    
- `%f` → For floating-point numbers
    

---

### 🧪 Examples
```go
fmt.Sprintf("I am %v years old", 10)
// "I am 10 years old" — %v prints the value directly

fmt.Sprintf("I am %s years old", "twenty")
// "I am twenty years old" — %s expects a string

fmt.Sprintf("I am %d years old", 25)
// "I am 25 years old" — %d expects an integer

fmt.Sprintf("I am %.2f years old", 10.523)
// "I am 10.52 years old" — %.2f rounds float to 2 decimal places
```
### 🤔 Why it feels less elegant than Python

In Python, you might write:
```python
f"I am {age} years old"
```
Much shorter and easier to read.

In Go, you write:
```go
fmt.Sprintf("I am %d years old", age)
```
A bit more manual and harder to remember.