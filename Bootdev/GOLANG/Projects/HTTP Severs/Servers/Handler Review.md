An [http.Handler](https://pkg.go.dev/net/http#Handler) is any [defined type](https://go.dev/ref/spec#Type_definitions) that implements the set of methods defined by the `Handler` [interface](https://go.dev/tour/methods/9), specifically the `ServeHTTP` method.

```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

The [ServeMux](https://pkg.go.dev/net/http#ServeMux) you used in the previous exercise is an `http.Handler`.

You will typically use a `Handler` for more complex use cases, such as when you want to implement a custom router, middleware, or other custom logic.

## HandlerFunc

```go
type HandlerFunc func(ResponseWriter, *Request)
```

You'll typically use a `HandlerFunc` when you want to implement a simple handler. The `HandlerFunc` type is just a function that matches the `ServeHTTP` signature above.

## Why This Signature?

The `Request` argument is fairly obvious: it contains all the information about the incoming request, such as the HTTP method, path, headers, and body.

The `ResponseWriter` is less intuitive in my opinion. The response is an _argument_, not a _return type_. Instead of returning a value all at once from the handler function, we _write_ the response to the `ResponseWriter`.

The question was:

> Which underlying type might implement the Handler interface?
> 
> - Any of these
> - An integer
> - A struct
> - A string

The correct answer is: **Any of these**

### Why?

In Go, **any type**—whether it's a built-in type like `int` or `string`, or a user-defined type like a `struct`—can implement an interface. An interface in Go is simply a set of method signatures. If a type (any type at all) has methods with the matching signatures, it is said to implement that interface.

Here’s an example with different underlying types all implementing the `Handler` interface (by defining the correct `ServeHTTP` method):

```go
// Underlying type: struct
type MyStruct struct{}
func (m MyStruct) ServeHTTP(w http.ResponseWriter, r *http.Request) {}

// Underlying type: int
type MyInt int
func (m MyInt) ServeHTTP(w http.ResponseWriter, r *http.Request) {}

// Underlying type: string
type MyString string
func (m MyString) ServeHTTP(w http.ResponseWriter, r *http.Request) {}
```

As you see, **any** of these could implement `Handler`, as long as they provide a suitable `ServeHTTP` method.

That’s why the answer is **“Any of these”**. In Go, interfaces don’t care about the type itself—only that the required methods are present.