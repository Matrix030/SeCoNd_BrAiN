The [chmod command](https://www.ibm.com/docs/en/aix/7.3?topic=c-chmod-command) lets you change the permissions of a file or directory. It's short for "change mode" (I wish it was called "change permissions", but alas).

## Assignment

As part of your security audit, you need to know who has access to the files in the `private` directory. The `ls` command has a `-l` option (lowercase "L") that will print out the permissions of each file and directory in long format.

1. [ ] List the files in the `worldbanc/private` directory along with their permissions using `ls -l` so you can see the current state of affairs.
2. [ ] Change the permissions of the `private` directory and all of its contents so that:
    - The owner can read, write, and execute
    - The group can do nothing
    - Others can do nothing

```bash
chmod -R u=rwx,g=,o= DIRECTORY
```

In the command above, `u` means "user" (aka "owner"), `g` means "group", and `o` means "others". The `=` means "set the permissions to the following", and the `rwx` means "read, write and execute". The `g=` and `o=` mean "set group and other permissions to nothing". The `-R` means "recursively", which means "do this to all of the contents of the directory as well".

Be sure to replace `DIRECTORY` with the path to the `private` directory.

_Remember, `.` is a special alias for the current directory._

3. [ ] Once you've changed the permissions, run the `ls -l` command again to make sure they've been updated.

Paste _only_ the **10-character** permission string of the updated `private` directory itself (not its contents) into the input field and submit your answer.

## Tip

If you're using WSL and `chmod` is not updating the permissions: your `worldbanc` directory may not be within your Linux subsystem. You can either move the directory or [adjust your wsl.conf file to allow for editing permissions](https://stackoverflow.com/questions/46610256/chmod-wsl-bash-doesnt-work).