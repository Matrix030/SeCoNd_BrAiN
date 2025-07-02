So if your terminal is just a program that lets you issue text-based commands and renders the output of those commands...

...What is the program that _runs_ those commands???

That's a shell.

Shells do a lot of things, but their main job is to interpret the commands you type and execute them.

## REPL

Shells are often referred to as "REPL"s. REPL stands for

- Read
- Eval (evaluate)
- Print
- Loop

This is a fancy way of saying that shells are programs that:

1. Read the commands you type
2. Evaluate those commands, usually by running _other_ programs on your computer
3. Print the output of those commands
4. Give you a new prompt to type another command and repeat