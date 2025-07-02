The `find` command is a powerful tool for finding files and directories by name, not by their contents.

## Find a File by Name

Let's say you're looking for a file named `hello.txt` somewhere in your home directory. You can use the `find` command to search for exactly that title:

```bash
find some_directory -name hello.txt
```

## Pattern Search

The `find` command can also search for files that match a pattern. For example, if you wanted to find all files that end in `.txt`, you could run:

```bash
find some_directory -name "*.txt"
```

The `*` character is a wildcard that matches anything. If you're trying to find filenames that _contain_ a specific word, you can use the `*` character to match the rest of the filename:

```bash
# Find all filenames that contain the word "chad"
find some_directory -name "*chad*"
```

## Assignment

You've been tipped off that fraudulent activity seems to happen often with "joint" accounts. "joint" means accounts that have more than one owner.

Use the `find` command to search the `worldbanc/public/products` directory for all files that contain the word "joint" in their name.

Paste the output of your `find` command into the input field and submit it.