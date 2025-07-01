Channels can be explicitly closed by a _sender_:

```go
ch := make(chan int)

// do some stuff with the channel

close(ch)
```

## Checking If a Channel Is Closed

Similar to the `ok` value when accessing data in a `map`, receivers can check the `ok` value when receiving from a channel to test if a channel was closed.

```go
v, ok := <-ch
```

ok is `false` if the channel is empty and closed.

## Don't Send on a Closed Channel

Sending on a closed channel will cause a panic. A panic on the main goroutine will cause the entire program to crash, and a panic in any other goroutine will cause _that goroutine_ to crash.

Closing isn't necessary. There's nothing wrong with leaving channels open, they'll still be garbage collected if they're unused. You should close channels to indicate explicitly to a receiver that nothing else is going to come across.