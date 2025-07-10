In C, we don't have a lot of abstractions at our disposal. There are no classes, destructors, functors, monads, made-up-category-theory-words, etc.

We've got _data_. And we've got _functions_. Just the way ~~God~~ Dennis Ritchie intended.

So, to make it easier to work with our `Stack`, we're going to build our own little `free` function that will clean up all the memory that we've allocated for our stack.