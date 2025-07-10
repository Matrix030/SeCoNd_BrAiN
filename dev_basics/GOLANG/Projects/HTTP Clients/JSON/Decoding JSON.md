When we receive JSON data in the body of an HTTP response, it comes as a stream of bytes. As we saw before, we _can_ just convert the bytes to a string... but in Go there's a better way. It's typically best to decode the JSON data into a _struct_.

Take this example JSON data:

```json
[
  {
    "id": "001-a",
    "title": "Unspaghettify code",
    "estimate": 9001
  }
]
```

To decode this JSON into a slice of `Issue` structs, we need to know the JSON fields and their types. The standard [`encoding/json`](https://pkg.go.dev/encoding/json) package uses tags to map JSON fields to struct fields.

Struct fields must be exported (capitalized) to decode JSON.

```go
type Issue struct {
	Id       string `json:"id"`
	Title    string `json:"title"`
	Estimate int    `json:"estimate"`
}
```

After receiving the response, we can decode it into a slice of `Issue`s with the "address of" operator `&`:

```go
// res is a successful `http.Response`

var issues []Issue
decoder := json.NewDecoder(res.Body)
if err := decoder.Decode(&issues); err != nil {
    fmt.Println("error decoding response body")
    return
}
```

If no error occurs, we can use the slice of items in our program.

```go
for _, issue := range issues {
    fmt.Printf("Issue – id: %v, title: %v, estimate: %v\n", issue.Id, issue.Title, issue.Estimate)
    // Issue – id: 001-a, title: Unspaghettify code, estimate: 9001
}
```