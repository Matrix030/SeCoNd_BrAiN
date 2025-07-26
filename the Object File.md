Let's take a look at what's inside this suspicious commit object file.

## Assignment

1. [ ] Try to [cat](https://man7.org/linux/man-pages/man1/cat.1.html) the contents of the commit object file (which will interpret the contents as text).

```bash
cat path/to/file/from-before
```

Nope... it's a mess. The contents have been compressed to raw bytes!

2. [ ] Try again with the [xxd](https://linux.die.net/man/1/xxd) command to print the contents of the file in hexadecimal format:

```bash
xxd path/to/file/from-before
```

You may need to install [xxd](https://linux.die.net/man/1/xxd).

3. [ ] Do that again, but redirect the output to a temporary file so the tests can read it:

```bash
xxd path/to/file/from-before > /tmp/commit_object_hex.txt
```

**Run and submit** the CLI tests.

## Hint

It's the file you used to answer the previous question...