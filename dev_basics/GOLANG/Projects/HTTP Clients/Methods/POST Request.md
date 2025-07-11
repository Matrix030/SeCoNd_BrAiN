An [HTTP POST request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) _sends_ data to a server, typically to _create_ a new resource.

## Adding a Body

The `body` of the request is the _payload_ sent to the server. The special `Content-Type` header is used to tell the server the format of the body: `application/json` for JSON data in our case. `POST` requests are generally _not_ safe methods to call multiple times because that would create duplicate records. For example, you wouldn't want to accidentally send a tweet twice.

Like `http.Get`, the standard library's [`http.Post`](https://pkg.go.dev/net/http#Post) function can be used to send simple `POST` requests. The trouble is, it's a bit limited. And because we need to add an `X-API-Key` header, the simple `http.Post` function won't work for us. Instead, we need to use [`http.NewRequest`](https://pkg.go.dev/net/http#NewRequest):

```go
type Comment struct {
	Id      string `json:"id"`
	UserId  string `json:"user_id"`
	Comment string `json:"comment"`
}

func createComment(url, apiKey string, commentStruct Comment) (Comment, error) {
    // encode our comment as json
	jsonData, err := json.Marshal(commentStruct)
	if err != nil {
		return Comment{}, err
	}

    // create a new request
	req, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
	if err != nil {
		return Comment{}, err
	}

    // set request headers
	req.Header.Set("Content-Type", "application/json")
    req.Header.Set("X-API-Key", apiKey)

    // create a new client and make the request
	client := &http.Client{}
	res, err := client.Do(req)
	if err != nil {
		return Comment{}, err
	}
	defer res.Body.Close()

    // decode the json data from the response
	// into a new Comment struct
	var comment Comment
	decoder := json.NewDecoder(res.Body)
	err = decoder.Decode(&comment)
	if err != nil {
		return Comment{}, err
	}

	return comment, nil
}
```