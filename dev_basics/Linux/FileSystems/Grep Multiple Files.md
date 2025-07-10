You can also search multiple files at once. For example, if we wanted to search for the word "hello" in `hello.txt` and `hello2.txt`, we could run:

```bash
grep "hello" hello.txt hello2.txt
```

## Recursive Search

You can also search an entire directory, including all subdirectories. For example, to search for the word "hello" in the current directory and all subdirectories:

```bash
grep -r "hello" .
```

_The `.` is a special alias for the current directory._

## Assignment

Use the grep command to find _all_ the lines with the text "CRITICAL" (all caps) in the `worldbanc/private/logs` _directory_.

Paste the output of your `grep` command into the input field and submit it.