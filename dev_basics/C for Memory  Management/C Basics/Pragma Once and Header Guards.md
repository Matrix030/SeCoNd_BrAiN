We saw how `.h` header files are used in a previous lesson, but before we go further let's talk about a potential issue you might run into: multiple inclusions. If the same header file gets included more than once, you can end up with some nasty errors caused by redefining things like functions or structs.

## Pragma Once

One simple solution (and the one we'll use for the rest of this course) is `#pragma once`. Adding this line to the top of a header file tells the compiler to include the file only once, even if it's referenced multiple times across your program.

```c
// my_header.h

#pragma once

struct Point {
    int x;
    int y;
};
```

## Header Guards

Another common way to avoid multiple inclusions is with include guards, which use preprocessor directives like this:

```c
#ifndef MY_HEADER_H
#define MY_HEADER_H

// some cool code

#endif
```

This method works by defining a unique [macro](https://gcc.gnu.org/onlinedocs/cpp/Macros.html) for the header file. If it’s already been included, the guard prevents it from being processed again.

Throughout this course, you’ll see `#pragma once` in our header files. It's quicker and less error-prone than traditional include guards, and it works well with most modern compilers.