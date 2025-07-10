Objects in C?!? No. Way.

_However_, Sneklang is built in C, and everything in Sneklang is an "object". To be clear, not a _class_ or _object-oriented programming_ object, but a higher-level data structure that _holds some metadata about itself_. For example, it will store:

- What type of data it holds (int, float, string, etc.)
- The size of the data it holds
- The data itself
- How many _references_ to itself exist (at least later, when we build our own garbage collector!)

That last point is critical! Because Sneklang is a garbage-collected language, we need to know how many references to an object exist so we can free it when it's no longer needed.