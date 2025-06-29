Remember that you can check if a key is already present in a map by using the second return value from the index operation.

You can combine an `if` statement with an assignment operation to use the variables inside the `if` block:

```go
names := map[string]int{}
missingNames := []string{}

if _, ok := names["Denna"]; !ok {
    // if the key doesn't exist yet,
    // append the name to the missingNames slice
    missingNames = append(missingNames, "Denna")
}
```

## Simpler Explaination

### How to Check if a Key Exists in a Map (in Go)

In Go, when you look up a value in a map, **you can also find out if the key actually exists**.
### Example:

```go
value, ok := myMap["someKey"]
```

- `value` gives you the value (if it exists)
- `ok` is a `bool` — it’s `true` if the key exists, `false` if it doesn’t

### Using This in an `if` Statement

You can write the check and use the result right inside an `if` statement:

```go
if _, ok := myMap["someKey"]; !ok {
    // Key doesn't exist
}
```

- `_` means “ignore the value” — we only care if the key is present
- `!ok` means the key is **not** in the map

### Full Example: Tracking Missing Names

Let’s say you’re checking if the name `"Denna"` is in a map:
```go
names := map[string]int{}
missingNames := []string{}

if _, ok := names["Denna"]; !ok {
    // Denna is not in the map, so add her to missingNames
    missingNames = append(missingNames, "Denna")
}
```

- We look up `"Denna"` in the map `names`
- If she's not there (`!ok`), we add her to the list of missing names
### Why This Is Useful

This pattern is **common in Go** when working with maps. It helps you:
- Avoid crashes from using non-existent keys
- Cleanly handle fallback behavior (like logging, appending, etc.)
