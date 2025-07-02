The `default` case in a `select` statement executes _immediately_ if no other channel has a value ready. A `default` case stops the `select` statement from blocking.

```go
select {
case v := <-ch:
    // use v
default:
    // receiving from ch would block
    // so do something else
}
```

## Tickers

- [time.Tick()](https://golang.org/pkg/time/#Tick) is a standard library function that returns a channel that sends a value on a given interval.
- [time.After()](https://golang.org/pkg/time/#After) sends a value once after the duration has passed.
- [time.Sleep()](https://golang.org/pkg/time/#Sleep) blocks the current goroutine for the specified duration of time.

The functions take a [`time.Duration`](https://pkg.go.dev/time#Duration) as an argument. For example:

```go
time.Tick(500 * time.Millisecond)
```

If you don't add `time.Millisecond` (or another unit), it will default to nanoseconds. That's — taking a wild guess here — probably faster than you want it to be.

## Read-Only Channels

A channel can be marked as read-only by casting it from a `chan` to a `<-chan` type. For example:

```go
func main() {
    ch := make(chan int)
    readCh(ch)
}

func readCh(ch <-chan int) {
    // ch can only be read from
    // in this function
}
```

## Write-Only Channels

The same goes for write-only channels, but the arrow's position moves.

```go
func writeCh(ch chan<- int) {
    // ch can only be written to
    // in this function
}
```