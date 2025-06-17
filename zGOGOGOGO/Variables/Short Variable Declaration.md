Sad variable declaration:

```go
var mySkillIssues int
mySkillIssues = 42
```

GOATed variable declaration:

```go
mySkillIssues := 42
```

The walrus operator, `:=`, declares a new variable and assigns a value to it in one line. Go can infer that `mySkillIssues` is an `int` because of the `42` value. Yay [type inference](https://en.wikipedia.org/wiki/Type_inference)!

## When to Use the Walrus Operator

The `:=`, (walrus operator) should be used instead of [var](https://go.dev/tour/basics/9) style declarations basically anywhere possible. The limitation is that `:=` can't be used outside of a function (in the [global/package scope](https://dave.cheney.net/2017/06/11/go-without-package-scoped-variables) which we'll talk about later).

Type inference is based on the value being assigned.

An `int`:

```go
mySkillIssues := 42
```

A `float64`:

```go
pi := 3.14159
```

A `string`:

```go
message := "Hello, world!"
```

A `bool`:

```go
isGoat := true
```
