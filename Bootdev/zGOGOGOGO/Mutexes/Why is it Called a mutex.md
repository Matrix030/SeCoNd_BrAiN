Mutex is short for [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion), and the conventional name for the data structure that provides it is "mutex", often abbreviated to "mu".

It's called "mutual exclusion" because a mutex _excludes_ different threads (or goroutines) from accessing the same data at the same time.