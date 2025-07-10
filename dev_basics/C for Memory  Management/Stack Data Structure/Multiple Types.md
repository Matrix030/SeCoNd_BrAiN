So we saw that we _can_ push both `int`and `int *` types into the same stack (again, a **bad idea**).

In this example, we'll see that not only can you push values and pointers into the same stack, but you can also push different types of values into the same stack.

This is once again a terrible idea, but the thrust of what we're getting at here is:

**C doesn't care what you put in memory, it's bytes all the way down.**

Fortunately, C doesn't let you do these conversions automatically... but it still lets you do them. When you're working with `void *`, you're working with raw memory addresses, and you can cast them to whatever you want.