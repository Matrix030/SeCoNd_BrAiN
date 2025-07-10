We can decode JSON bytes (or strings) into a Go struct using [`json.Unmarshal`](https://pkg.go.dev/encoding/json#Unmarshal) or a [`json.Decoder`](https://pkg.go.dev/encoding/json#Decoder).

The `Decode` method of `json.Decoder` streams data from an [`io.Reader`](https://pkg.go.dev/io#Reader) into a Go struct, while `json.Unmarshal` works with data that's already in `[]byte` format. Using a `json.Decoder` can be more memory-efficient because it doesn't load all the data into memory at once. `json.Unmarshal` is ideal for small JSON data you already have in memory. When dealing with HTTP requests and responses, you will likely use `json.Decoder` since it works directly with an `io.Reader`.

## Example

```go
// res is an http.Response
defer res.Body.Close()

data, err := io.ReadAll(res.Body)
if err != nil {
	return nil, err
}

var issues []Issue
if err := json.Unmarshal(data, &issues); err != nil {
    return nil, err
}
```