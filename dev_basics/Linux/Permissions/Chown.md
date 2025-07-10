So when do you _need_ to use `sudo`? `chmod` allows you to change the permissions of any file or directory that _you_ own. But what if you don't own the file or directory? That's where `sudo` is required. Let's change ownership of a directory to see how that works.

The `chown` command, which stands for "change owner", allows you to change the owner of a file or directory, and it requires root privileges.

## Assignment

1. [ ] Change directory into `worldbanc/private`. Take a look at the contents with `ls`.
2. [ ] You should see a directory called `contacts`. Take a look at its contents.
3. [ ] You should see a file called `emergency.txt`. Take a look at the data inside with `cat`.

Hmmm, it seems like it has some personal contact info for employees. We don't know whose this is, but we're pretty sure it should have its access strictly limited.

4. [ ] Let's change the owner of the entire `contacts` directory to `root`. Run this command in `worldbanc/private` directory:

```bash
sudo chown -R root contacts
```

Here's an explanation of the command:

- `sudo` - Run as the root user
- `chown` - Command to change the owner
- `-R` - "Recursive", meaning also apply the changes to everything inside the directory
- `root` - The name of the new owner
- `contacts` - The directory to change the owner of

5. [ ] From the `worldbanc/private` directory, run:

```bash
ls -l
```

Paste only the line from the output that represents the `contacts` directory into the input field and submit your answer.