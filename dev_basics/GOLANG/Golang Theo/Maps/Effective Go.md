Read the following paraphrased sections from [effective Go regarding maps](https://go.dev/doc/effective_go#maps):

## Like Slices, Maps Hold References

Like slices, maps hold references to an underlying data structure. If you pass a map to a function that changes the contents of the map, the changes will be visible in the caller.

## Map Literals

Maps can be constructed using the usual composite literal syntax with colon-separated key-value pairs, so it's easy to build them during initialization.

```go
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

## Missing Keys

An attempt to fetch a map value with a key that is not present in the map will return the zero value for the type of the entries in the map. For instance, if the map contains integers, looking up a non-existent key will return 0. A set can be implemented as a map with value type bool. Set the map entry to true to put the value in the set, and then test it by simple indexing.

```go
attended := map[string]bool{
    "Ann": true,
    "Joe": true,
    ...
}

if attended[person] { // will be false if person is not in the map
    fmt.Println(person, "was at the meeting")
}
```

Sometimes you need to distinguish a missing entry from a zero value. Is there an entry for "UTC" or is that 0 because it's not in the map at all? You can discriminate with a form of multiple assignment.

```go
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```

For obvious reasons, this is called the “comma ok” idiom. In this example, if tz is present, seconds will be set appropriately and ok will be true; if not, seconds will be set to zero and ok will be false. Here's a function that puts it together with a nice error report:

```go
func offset(tz string) int {
    if seconds, ok := timeZone[tz]; ok {
        return seconds
    }
    log.Println("unknown time zone:", tz)
    return 0
}
```

Now we can redo the first example of a set by using a more efficient map with empty structs, which don't take up any space in memory:

```go
attended := map[string]struct{}{
    "Ann": {},
    "Joe": {},
    ...
}

if _, ok := attended[person]; ok {
    fmt.Println(person, "was at the meeting")
}
```

## Deleting Map Entries

To delete a map entry, use the delete built-in function, whose arguments are the map and the key to be deleted. It's safe to do this even if the key is already absent from the map.

```go
delete(timeZone, "PDT")  // Now on Standard Time
```

## Simpler Explaination

Here’s a simplified, beginner-friendly explanation of each section from the **Effective Go** guide on maps:

---

### 🔁 Maps Are Like Slices — They Hold References

When you pass a **map** to a function, **you’re passing a reference**, not a copy. So if the function changes the map, **those changes are visible outside the function too**.

Think of it like sharing a whiteboard — if one person writes on it, everyone sees the change.

---

### 🧱 Map Literals — Easy Initialization

You can create a map and fill it with key-value pairs right away using a **map literal**:

```go
var timeZone = map[string]int{
    "UTC":  0,
    "EST": -5 * 3600,
    "PST": -8 * 3600,
}
```

This makes it super quick to define small maps, like time zone offsets or settings.

---

### ❓ Missing Keys Return Zero Values

If you try to get a value from a map using a key that doesn’t exist, **Go won’t panic**. Instead, it just returns the **zero value** for that type:

- For `int`, it returns `0`
    
- For `bool`, it returns `false`
    
- For `string`, it returns `""`
    

**Example:**

```go
fmt.Println(timeZone["Mars"]) // 0, because key doesn't exist
```

So how can we **check if the key actually existed**?

---

### ✅ The “Comma ok” Idiom

Use this pattern to check whether a key was found in the map:

```go
value, ok := myMap["someKey"]
```

- `ok` is `true` if the key exists
    
- `false` if it doesn’t
    

**Example:**

```go
seconds, ok := timeZone["UTC"]
```

If `"UTC"` is in the map:

- `seconds` will have the correct value
    
- `ok` will be `true`
    

If not:

- `seconds` will be `0`
    
- `ok` will be `false`
    

---

### ✅✅ Using a Map as a Set

You can create a **set** (a collection of unique items) using a map.

#### Basic version:

```go
attended := map[string]bool{
    "Ann": true,
    "Joe": true,
}
```

To check if someone attended:

```go
if attended["Ann"] {
    fmt.Println("Ann was at the meeting")
}
```

#### Even better (more memory efficient):

Use `struct{}` as the value — it takes **zero memory**.

```go
attended := map[string]struct{}{
    "Ann": {},
    "Joe": {},
}
```

Check for presence:

```go
if _, ok := attended["Ann"]; ok {
    fmt.Println("Ann was at the meeting")
}
```

---

### ❌ Deleting a Map Entry

To remove a key from a map, use the built-in `delete()` function:

```go
delete(timeZone, "PDT")
```

If the key isn’t in the map, **nothing bad happens** — Go just ignores it.

---

### Summary (Quick Cheatsheet)

|Task|How to Do It|
|---|---|
|Check if a key exists|`value, ok := m[key]`|
|Delete a key|`delete(m, key)`|
|Use a map as a set|`map[string]struct{}{}`|
|Modify map in a function|Just pass it — it’s a reference|
|Create a map with values|Use map literals (`map[Key]Val{}`)|

Let me know if you'd like this in Obsidian format!