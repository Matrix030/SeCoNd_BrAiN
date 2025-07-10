Constants must be known at compile time. They are _usually_ declared with a static value:

```go
const myInt = 15
```

However, constants _can be computed_ as long as the computation can happen at _compile time_.

For example, this is valid:

```go
const firstName = "Lane"
const lastName = "Wagner"
const fullName = firstName + " " + lastName
```

That said, you _cannot_ declare a constant that can only be computed at run-time like you can in JavaScript. This breaks:

```go
// the current time can only be known when the program is running
const currentTime = time.Now()
```