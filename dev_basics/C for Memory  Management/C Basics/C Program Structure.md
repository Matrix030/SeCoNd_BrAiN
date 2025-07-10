In Python you'd do something like this:

```sh
python slow_program.py
```

The Python interpreter then executes that file top-to-bottom. If you have a `print()` at the top level, then it will print something.

The entire file is _interpreted line by line_, _but that's not how C works_.

## Simplest C Program

The simplest C program is essentially:

```c
int main() {
    return 0;
}
```

But a lot is happening here...

- A function named `main` is _always_ the entry point to a [C program](https://www.youtube.com/watch?v=tas0O586t80) (unlike Python, which enters at the top of the file).
- `int` is the return type of the function and is short for "integer". Because this is the `main` function, the return value is the [exit code](https://en.wikipedia.org/wiki/Exit_status) of the program. `0` means success, anything else means failure.
    - You'll find a lot of abbreviations in C because 1) programmers are lazy, and 2) it used to matter how many bytes your source code was.
- The opening bracket, `{` is the start of the function's body (C ignores whitespace, so indentation is just for style, not for syntax)
- `return 0` returns the `0` value (an integer) from the function. Again, this is the exit code because it's the `main` function.
    - `0` represents "nothing bad happened" as a return value.
- The pesky `;` at the end of `return 0;` is required in C to terminate statements.
- The closing bracket, `}` denotes the end of the function's body.

## Print

It feels very different coming from Python, but printing in C is done with a function called `printf` from the [`stdio.h`](https://www.ibm.com/docs/en/zos/2.4.0?topic=files-stdioh-standard-input-output) (standard input/output) library with a lot of weird formatting rules. To use it, you need an `#include` at the top of your file:

```c
#include <stdio.h>
```

Then you can use [`printf`](https://devdocs.io/c/io/fprintf) from inside a function:

```c
printf("Hello, world!\n");
```

_Notice the `\n`: it's required to print a [newline character](https://en.wikipedia.org/wiki/Newline) (and flush the buffer in the browser), which `print()` in Python does automatically._

_In case you're wondering, the `f` in `printf` stands for "print formatted"._