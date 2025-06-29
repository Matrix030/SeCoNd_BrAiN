## Insert an Element

```go
m[key] = elem
```

## Get an Element

```go
elem = m[key]
```

## Delete an Element

```go
delete(m, key)
```

## Check If a Key Exists

```go
elem, ok := m[key]
```

- If `key` is in `m`, then `ok` is `true` and `elem` is the value as expected.
- If `key` is not in the map, then `ok` is `false` and `elem` is the zero value for the map's element type.