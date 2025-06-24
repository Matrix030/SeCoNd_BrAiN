For the previous `swap` implementations we've known the type of the data we want to swap. Because we knew the type, the compiler knew the sizes of the data we want to swap.

However, to make a generic swap, we will need to provide the C compiler with the size of the data that we are swapping because `void *` loses that type info. Our new interface for `swap` will include the size:

```c
void swap(void *vp1, void *vp2, size_t size);
```

The other problem we're going to have is that directly assigning pointer values does not work the same way with `void *`. Instead of using `*ptr1 = *ptr2`, we will use [`memcpy`](https://en.cppreference.com/w/c/string/byte/memcpy), which is included in the `string.h` library:

```c
void *memcpy(void *destination, void* source, size_t size);
```

So to move the data from `ptr2` to `ptr1`, we will use the following:

```c
memcpy(ptr1, ptr2, size);
```