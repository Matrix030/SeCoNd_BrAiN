Up until now, even though we made our stack with `void *`, you'll notice that we've only stored plain old `int` pointers. I want to show you that you can actually store _anything_ in the stack, even heterogeneous lists! That being said, this is usually a bad idea.

TJ likes to use words like "heterogeneous" to keep kids-who-dont-read-good at bay. He meant to say, "you can actually store _anything_ in the stack, even lists of different types of data!"

Now, we're going to do something _pretty gross_ to demonstrate the wise words of one of the philosophers of our time:

> With great power comes great responsibility.
> -- Uncle Ben

I'm going to have you push an `int *`and a regular old `int` directly onto the stack (a bad idea). I just want to show you that you can store _anything_ in a `void *`, even values that aren't pointers at all.