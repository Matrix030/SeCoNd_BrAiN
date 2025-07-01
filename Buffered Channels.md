Channels can _optionally_ be buffered.

## Creating a Channel With a Buffer

You can provide a buffer length as the second argument to `make()` to create a buffered channel:

```go
ch := make(chan int, 100)
```

A buffer allows the channel to hold a fixed number of values before sending blocks. This means sending on a buffered channel only blocks when the buffer is full, and receiving blocks only when the buffer is empty.