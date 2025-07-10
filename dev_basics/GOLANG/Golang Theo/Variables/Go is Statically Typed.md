Go enforces [static typing](https://developer.mozilla.org/en-US/docs/Glossary/Static_typing) meaning variable types are known _before_ the code runs. That means your editor and the compiler can display type errors before the code is ever run, making development easier and faster.

Contrast this with most dynamically typed languages like JavaScript and Python... Dynamic typing often leads to subtle bugs that are hard to detect. The code _must_ be run to catch syntax and type errors. (sometimes in production if you're unlucky 😨)

Languages also have [strong or weak typing](https://en.wikipedia.org/wiki/Strong_and_weak_typing), meaning stricter or weaker type checking rules.

## Concatenating Strings

Two strings can be [concatenated](https://en.wikipedia.org/wiki/Concatenation) with the `+` operator. But the compiler will not allow you to concatenate a `string` variable with an `int` or a `float64`.