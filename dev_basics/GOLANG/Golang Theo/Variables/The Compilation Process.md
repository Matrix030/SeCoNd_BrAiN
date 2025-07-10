Computers need machine code, they don't understand English or even Go. We need to convert our high-level (Go) code into machine language, which is really just a set of instructions that some specific hardware can understand. In your case, your CPU.

The Go compiler's job is to take Go code and produce machine code, an `.exe` file on Windows or a standard executable on Mac/Linux.

![compiler](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/rfR5MNc.png)

## Go Program Structure

We'll go over this all later in more detail, but to sate your curiosity:

1. `package main` lets the Go compiler know that we want this code to compile and run as a standalone program, as opposed to being a library that's imported by other programs.
2. `import "fmt"` imports the [`fmt` (formatting) package](https://pkg.go.dev/fmt) from the [standard library](https://pkg.go.dev/std). It allows us to use `fmt.Println` to print to the console.
3. `func main()` defines the `main` function, the entry point for a Go program.

## Two Kinds of Bugs

Generally speaking, there are two kinds of errors in programming:

1. **Compilation errors.** Occur when code is compiled. It's generally better to have compilation errors because they'll never accidentally make it into production. You can't ship a program with a compiler error because the resulting executable won't even be created.
2. **Runtime errors.** Occur when a program is running. These are generally worse because they can cause your program to crash or behave unexpectedly.