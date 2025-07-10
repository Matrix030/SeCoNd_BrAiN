In the previous lesson, we saw how we can receive values from channels like this:

```go
v := <-ch
```

However, sometimes we don't care _what_ is passed through a channel. We only care _when_ and _if_ something is passed. In that situation, we can block and wait until something is sent on a channel using the following syntax.

```go
<-ch
```

This will block until it pops a single item off the channel, then continue, discarding the item.

In cases like this, [Empty structs](https://dave.cheney.net/2014/03/25/the-empty-struct) are often used as a [unary](https://en.wikipedia.org/wiki/Unary_operation) value so that the sender communicates that this is only a "signal" and not some data that is meant to be captured and used by the receiver.

Here is an example:

```go
func downloadData() chan struct{} {
	downloadDoneCh := make(chan struct{})

	go func() {
		fmt.Println("Downloading data file...")
		// after the download is done, send a "signal" to the channel
		downloadDoneCh <- struct{}{}
	}()

	return downloadDoneCh
}

func processData(downloadDoneCh chan struct{}) {
	// any code here can run normally
	fmt.Println("Preparing to process data...")

	// block until `downloadData` sends the signal that it's done
	<-downloadDoneCh

	// any code here can assume that data download is complete
	fmt.Println("Data download ensured, starting data processing...")
}
```