#cool_stuff

Any type can be used as the _value_ in a map, but _keys_ are more restrictive.

Read the following section of the official [Go blog](https://go.dev/blog/maps):

As mentioned earlier, **map keys may be of any type that is comparable**. The language spec defines this precisely, but in short, comparable types are boolean, numeric, string, pointer, channel, and interface types, and structs or arrays that contain only those types. Notably absent from the list are slices, maps, and functions; these types cannot be compared using ==, and may not be used as map keys.

It's obvious that strings, ints, and other basic types should be available as map keys, but perhaps unexpected are struct keys. Struct can be used to key data by multiple dimensions. For example, this map of maps could be used to tally web page hits by country:

```go
hits := make(map[string]map[string]int)
```

This is a map of string to (map of string to int). Each key of the outer map is the path to a web page with its own inner map. Each inner map key is a two-letter country code. This expression retrieves the number of times an Australian has loaded the documentation page:

```go
n := hits["/doc/"]["au"]
```

Unfortunately, this approach becomes unwieldy when adding data, as for any given outer key you must check if the inner map exists, and create it if needed:

```go
func add(m map[string]map[string]int, path, country string) {
    mm, ok := m[path]
    if !ok {
        mm = make(map[string]int)
        m[path] = mm
    }
    mm[country]++
}
add(hits, "/doc/", "au")
```

On the other hand, a design that uses a single map with a struct key does away with all that complexity:

```go
type Key struct {
    Path, Country string
}
hits := make(map[Key]int)
```

When a Vietnamese person visits the home page, incrementing (and possibly creating) the appropriate counter is a one-liner:

```go
hits[Key{"/", "vn"}]++
```

And it’s similarly straightforward to see how many Swiss people have read the spec:

```go
n := hits[Key{"/ref/spec", "ch"}]
```


## Simpler Explaination

In Go, **you can store anything as a value in a map**, but **not all types can be used as keys**. The rules for keys are stricter.

### What Can Be a Map Key?

A **map key must be a type that Go can compare using `==`**, like:

- `int`, `float`, `string`, `bool`
- `pointers`, `channels`, and `interfaces`
- **structs or arrays** — but **only if** they contain _only_ the above types

> ❌ You **cannot** use slices, maps, or functions as keys, because Go can't compare them using `==`.

### Example: Nested Maps

Let’s say you want to track how many people from each country visited a specific web page. One way is to use **a map inside another map**: