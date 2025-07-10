A program is just a set of instructions that a computer can execute, and an "executable" is just a file that contains a program. The words "program" and "executable" are often used interchangeably. Broadly speaking, there are two types of programs:

- Compiled programs
- Interpreted programs

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: Compiled vs interpreted programs

## Compiled Programs

A compiled program is a program that has been converted from human-readable source code into machine code (binary). [Machine code](https://en.wikipedia.org/wiki/Machine_code) is a set of instructions that a computer can execute directly: your computer's CPU is hardware that's been designed to execute machine code.

Programming languages like [Go](https://go.dev/), [C](https://en.wikipedia.org/wiki/C_\(programming_language\)), and [Rust](https://www.rust-lang.org/) produce compiled programs.

## Interpreted Programs

An interpreted program is a program that is executed by _another_ program. The program that executes the interpreted program is called an [interpreter](https://en.wikipedia.org/wiki/Interpreter_%28computing%29). The interpreter reads the source code of the interpreted program and executes it.

Programming languages like [Python](https://www.python.org/), [Ruby](https://www.ruby-lang.org/en/), and [JavaScript](https://en.wikipedia.org/wiki/JavaScript), are typically interpreted as they run, which means your computer needs to have the interpreter installed to run the program.

Another example is the `.sh` shell script files we talked about. Those are interpreted by the shell program.

## Assignment

1. [ ] In your shell, run the following command:

```bash
which sh
```

The `which` command tells you the location of an installed command line program. In this case, we're asking for the location of the `sh` (shell) program. Mine is located at `/bin/sh`.

2. [ ] Take a look at the contents of that file:

```bash
cat /bin/sh
```

Keep in mind, your `sh` program is a compiled executable, probably written in C. That's why when you try to view its contents with `cat`, you see... what you see.

A file with a `.sh` extension on the other hand is a shell script. It's a text file that contains commands that will be interpreted and run by the `sh` program. They are both executable programs, but only one can be run without the help of another program.

**Answer the associated questions**.