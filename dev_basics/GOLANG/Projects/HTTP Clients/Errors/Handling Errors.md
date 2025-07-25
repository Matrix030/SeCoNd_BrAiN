When making HTTP requests in Go, it is essential to correctly handle both network errors and non-OK responses from the server. Proper error handling ensures your application can gracefully manage issues and provide meaningful feedback.

For the sake of simplicity, all previous lessons have assumed that all our requests have resulted in successful responses (status code `200` — `299`).

## Network Errors

Network errors happen when there are problems reaching the server, like DNS failures, connectivity issues, or timeouts. You can detect these errors by checking the `error` returned by the HTTP request function.

```go
res, err := http.Get("https://example.com/api/resource")
if err != nil {
    log.Printf("Network error: %v", err)
    return
}
defer res.Body.Close()
```

## Non-Ok Responses

Even if the request is successful, the server may return a non-OK HTTP status code (e.g., 404 Not Found, 500 Internal Server Error). These responses need to be handled _separately_ from network errors.

```go
res, err := http.Get("https://example.com/api/resource")
if err != nil {
    fmt.Println("a network error occurred")
    return
}
defer res.Body.Close()

if res.StatusCode != http.StatusOK {
    fmt.Println("status code != 200")
    return
}
```

A successful response usually has a status code of `http.StatusOK` (200), but it can be any code between `200` and `299`.

What you should do with a non-OK response for your request depends on... you. You may choose to return an error, try the request again, or just log the error.