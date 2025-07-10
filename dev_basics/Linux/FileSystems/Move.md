The [move command](https://www.ibm.com/docs/en/aix/7.3?topic=files-moving-renaming-mv-command) moves a file or directory from one location to another. You can use it to rename a file or to move it to a different directory altogether. Your working directory can't be the directory you're moving.

Renaming a file:

```bash
mv some_file.txt some_other_name.txt
```

Moving a file from the current directory to another nested directory:

```bash
mv some_file.txt some_directory/some_file.txt
```

Moving a file from the current directory, to the parent directory:

```bash
mv some_file.txt ../some_file.txt
```

If you don't want to rename the file and you're just moving it to a different directory, you can omit the filename:

```bash
mv some_file.txt some_directory/
```

## Assignment

Move the `tbills.txt` file from `worldbanc/public/products/credit_cards/tbills.txt`, into the new `worldbanc/public/products/investments` directory. Keep the filename the same.

List the contents of the `investments` directory and paste the output into the text field and submit your answer.

Both the target and destination have to be valid paths from the current working directory. Use `pwd` to see where you are and adjust the source and destination paths accordingly. Use `ls` to verify that the file has been moved correctly. If you mess up the `mv` command, you'll need to figure out where you accidentally moved the file to, then move it from that location back to where it belongs.