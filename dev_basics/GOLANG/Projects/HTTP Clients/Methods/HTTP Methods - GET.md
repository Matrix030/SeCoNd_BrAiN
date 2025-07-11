HTTP defines a set of [methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). We must choose one to use each time we make an HTTP request. The most common ones include:

- `GET`
- `POST`
- `PUT`
- `DELETE`

The [GET method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) is used to "get" a _representation_ of a specified resource. It doesn't _take_ (remove) the data from the server but rather _gets_ a representation, or copy, of the resource in its current state. A GET request is considered a [_safe_](https://developer.mozilla.org/en-US/docs/Glossary/Safe/HTTP) method to call multiple times because it shouldn't alter the state of the server.

## Making a GET Request in Go

There are two basic ways to make a Get request in Go.

1. The simple but less powerful way: [`http.Get`](https://pkg.go.dev/net/http#Get)
2. The verbose but more powerful way: [`http.Client`](https://pkg.go.dev/net/http#Client), [http.NewRequest](https://pkg.go.dev/net/http#NewRequest), and [http.Client.Do](https://pkg.go.dev/net/http#Client.Do)

If all you need to do is make a simple `GET` request to a URL, `http.Get` will work:

```go
resp, err := http.Get("https://jsonplaceholder.typicode.com/users")
```

If you need to customize things like headers, cookies, or timeouts, you'll want to create a custom [`http.Client`](https://pkg.go.dev/net/http#Client), and [`http.NewRequest`](https://pkg.go.dev/net/http#NewRequest), then use the client's [`Do`](https://pkg.go.dev/net/http#Client.Do) method to execute it.

```go
client := &http.Client{
	Timeout: time.Second * 10,
}

req, err := http.NewRequest("GET", "https://jsonplaceholder.typicode.com/users", nil)
if err != nil {
	log.Fatal(err)
}

resp, err := client.Do(req)
```