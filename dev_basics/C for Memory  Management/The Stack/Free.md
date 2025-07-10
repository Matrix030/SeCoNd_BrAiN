The [`free`](https://en.cppreference.com/w/c/memory/free) function deallocates memory that was previously allocated by [`malloc`](https://en.cppreference.com/w/c/memory/malloc), [`calloc`](https://en.cppreference.com/w/c/memory/calloc), or [`realloc`](https://en.cppreference.com/w/c/memory/realloc).

**IMPORTANT**: `free` does not change the **value** stored in the memory, and it doesn't even change the address stored in the pointer. Instead, it simply informs the Operating System that the memory can be used again.

## Forgetting to free

Forgetting to call `free` leads to a memory leak. This means that the allocated memory remains occupied and cannot be reused, even though the program no longer needs it. Over time, if a program continues to allocate memory without freeing it, the program may run out of memory and crash.

Memory leaks are one of the most common bugs in C programs, and they can be difficult to track down because the memory is still allocated and accessible, even though it is no longer needed.