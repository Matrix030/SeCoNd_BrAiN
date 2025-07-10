If there is a way to [unmarshal](https://pkg.go.dev/encoding/json#Unmarshal) JSON data, there must be a way to marshal it as well. The [`json.Marshal`](https://pkg.go.dev/encoding/json#Marshal) function converts a Go struct into a slice of bytes representing JSON data.

## Example

```go
type Board struct {
	Id       int    `json:"id"`
	Name     string `json:"name"`
	TeamId   int    `json:"team"`
	TeamName string `json:"team_name"`
}

board := Board{
	Id:       1,
	Name:     "API",
	TeamId:   9001,
	TeamName: "Backend",
}

data, err := json.Marshal(board)
if err != nil {
	log.Fatal(err)
}
fmt.Println(string(data))
// {"id":1,"name":"API","team":9001,"team_name":"Backend"}

```