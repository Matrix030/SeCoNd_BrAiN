The `cd` command "changes directory" to move _into_ a directory. But how do you move back _out_ of the current directory?

The answer is two dots: `..`

`..` is a special alias that refers to "the parent directory". It's a shortcut that you can use to move up one level in the directory tree.

## Assignment

Navigate back out of the `worldbanc` directory so that you're in its parent directory. If you're in the top level of the `worldbanc` directory, just type:

```bash
cd ..
```

Then run the `ls` command again, but pass in the name of the `worldbanc` directory as an argument to list its contents from the parent directory:

```bash
ls worldbanc
```