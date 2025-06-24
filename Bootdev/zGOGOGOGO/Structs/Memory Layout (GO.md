In Go, structs sit in memory in a contiguous block, with fields placed one after another as defined in the struct. For example this struct:

```go
type stats struct {
	Reach    uint16
	NumPosts uint8
	NumLikes uint8
}
```

Looks like this in memory:

![struct layout](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/X8Vr5mS.png)

## Field ordering... Matters?

the order of fields in a struct can have a big impact on memory usage. This is the same struct as above, but _poorly_ designed:

```go
type stats struct {
	NumPosts uint8
	Reach    uint16
	NumLikes uint8
}
```

It looks like this in memory:

![struct layout](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/BaBaUjQ.png)

Notice that Go has "aligned" the fields, meaning that it has added some padding (wasted space) to make up for the size difference between the `uint16` and `uint8` types. It's done for execution speed, but it can lead to increased memory usage.

## Should I Panic?

To be honest, you _should not stress_ about [memory layout](https://go101.org/article/memory-layout.html). However, if you have a specific reason to be concerned about memory usage, aligning the fields by size (largest to smallest) can help. You can also use the [reflect package](https://pkg.go.dev/reflect) to debug the memory layout of a struct:

```go
typ := reflect.TypeOf(stats{})
fmt.Printf("Struct is %d bytes\n", typ.Size())
```

## Real Story

I once had a server in production that held _a lot_ of structs in memory. Like _hundreds of thousands_ in a list. When I re-ordered the fields in the struct, the memory usage of the program dropped by over 2 gigabytes! It was a _huge_ performance win.