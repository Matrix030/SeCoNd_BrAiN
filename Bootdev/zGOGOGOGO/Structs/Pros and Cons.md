To solve our cyclic reference issue (and to force you to implement another GC algorithm, HA!) we're going to implement a [Mark and Sweep](https://en.wikipedia.org/wiki/Tracing_garbage_collection#Na%C3%AFve_mark-and-sweep) garbage collector.

## Pros of MaS

- Can detect cycles, and thus prevent memory leaks in certain cases
- Less on-demand bookkeeping (Remember: all work done by the GC is "wasted" - it doesn't make your ~~GPT-4 wrapper~~ custom AI product run any faster)
- Reduces potential performance degradation in highly multithreaded programs (refcounting requires atomic updates for thread safety)

## Cons of MaS

- More complex to implement (heh, you'll see)
- Can cause "stop-the-world" pauses when lots of objects exist and must be freed (resulting in poor performance)
- Higher memory overhead
- Less predictable performance