One of the simplest ways to implement a garbage collector is to use a [reference counting](https://en.wikipedia.org/wiki/Garbage_collection_\(computer_science\)#Reference_counting) algorithm. It goes something like this:

- All objects keep track of a `reference_count` integer.
- When that object is referenced, its reference count is incremented.
- When an object is garbage collected, the reference count of any object it references is decremented.
- When any object's reference count reaches zero, the object is garbage collected.