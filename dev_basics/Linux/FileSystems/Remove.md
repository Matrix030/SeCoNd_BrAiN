The [remove command](https://www.ibm.com/docs/en/aix/7.3?topic=files-deleting-rm-command) deletes a file or empty directory:

```bash
rm some_file.txt
```

You can optionally add a `-r` flag to tell the `rm` command to delete a directory and _all_ of its contents recursively. "Recursively" is just a fancy way of saying "do it again on all of the subdirectories and their contents".

```bash
rm -r some_directory
```

## Assignment

1. [ ] Change directories to the `worldbanc/private` directory using the `cd` command.
2. [ ] Use the `cat` command to view the contents of the `worldbanc/private/passwords/passwords.txt` file.

There's a big problem here! It looks like someone is storing passwords in plain text! Even on a private machine, this is a big security risk.

3. [ ] Delete both the `passwords` directory and the `passwords.txt` file within it.
4. [ ] List the contents of the "private" directory again to make sure that the file is gone.

Paste the contents of the `private` directory into the input field and submit it.