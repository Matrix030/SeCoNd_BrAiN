Understanding how your _software_ runs on _hardware_ is important for writing fast, performant code. In this course we'll be talking all about one of the main aspects of software performance: **memory management**.

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: Welcome To Memory Management

## Goals of This Course

1. **Understand how and where programs store data in memory**. Variables, functions and objects don't get to exist for free. Where do they live as your code runs?
2. **Learn how to make programs more efficient**. Most performance related problems in backend software are memory related (at least in my experience). Learn how it all works so you can troubleshoot and optimize.
3. **Practice programming in a lower-level language**. C gets you much closer to the hardware than Python, JavaScript, or Go. By writing C, you'll learn a lot about how software works closer to the metal.
4. **Learn about garbage collection (and build your own)**. You will likely work in a garbage collected language at some point, whether that's Python, Go, JavaScript or something else. Best to understand what trade-offs are being made.

This is not a C course. We will be writing C code, and we'll cover the basics of C that you'll need, but we're focusing on memory, not C.

## Prerequisites

- **Coding experience**: You should be comfortable writing code in at least one other language, like Python, JavaScript, or Go.
- **CS Basics**: We expect that you understand basic algorithms, data structures, OOP and FP concepts.

## Building Sneklang

If you're familiar with super high-level languages, you're probably used to _not_ thinking about memory. In this course, we'll be **building the "Sneklang" programming language** (okay just little parts of it) **in C**. We will study all about manually managing memory in C, and toward the end of the course, we'll build a simple garbage collector so that Sneklang devs don't have to think too hard.