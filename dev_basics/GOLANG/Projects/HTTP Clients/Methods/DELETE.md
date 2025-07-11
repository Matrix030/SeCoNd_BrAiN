The `DELETE` method does exactly what you expect: it deletes a specified resource.

## Example

```go
// This deletes the location with ID: 52fdfc07-2182-454f-963f-5f0f9a621d72
url := "https://api.boot.dev/v1/courses_rest_api/learn-http/locations/52fdfc07-2182-454f-963f-5f0f9a621d72"
req, err := http.NewRequest("DELETE", url, nil)
if err != nil {
	fmt.Println(err)
    return
}

client := &http.Client{}
res, err := client.Do(req)
if err != nil {
	fmt.Println(err)
    return
}
defer res.Body.Close()

if res.StatusCode > 299 {
    fmt.Println("request to delete location unsuccessful")
    return
}
fmt.Println("request to delete location successful")
```