The standard library also exposes a [sync.RWMutex](https://golang.org/pkg/sync/#RWMutex)

In addition to these methods:

- [Lock()](https://golang.org/pkg/sync/#Mutex.Lock)
- [Unlock()](https://golang.org/pkg/sync/#Mutex.Unlock)

The `sync.RWMutex` also has these methods for concurrent reads:

- [RLock()](https://golang.org/pkg/sync/#RWMutex.RLock)
- [RUnlock()](https://golang.org/pkg/sync/#RWMutex.RUnlock)

The `sync.RWMutex` improves performance in read-intensive processes. Multiple goroutines can safely read from the map simultaneously, as many `RLock()` calls can occur at the same time. However, only one goroutine can hold a `Lock()`, and during this time, all `RLock()` operations are blocked.