You're familiar with the idea of reading and writing data into files. But what about _executing_ them? Executable files are just files where the data stored inside is a program that you can run on your computer.

Files with a `.sh` extension are [shell scripts](https://en.wikipedia.org/wiki/Shell_script). They're just text files that contain shell commands. You can run a file in your shell by typing its filepath:

```bash
mydir/program.sh
```

Interestingly, if the program is in the current directory (in this example, the `mydir` directory), you need to prefix it with `./` to run it:

```bash
./program.sh
```

As far as file paths go, `./program.sh` and `program.sh` are the same. The dot (`.`) is an alias for the current directory. We _need_ the prefix when running executables so that the shell knows we're trying to run a file from a file path, not an installed command like `ls`, `mkdir`, `chmod`, etc.

## Assignment

There seems to be a suspicious script in the `worldbanc/private/bin` directory called `genids.sh`. It looks like it generates some type of identifier... First, let's **remove our ability to run it**, just to see what happens. The `chmod` command has a convenient `-x` flag that will simply remove the executable permission from the file.

1. [ ] Run the following commands in `worldbanc/private/bin`:

```bash
chmod -x genids.sh
```

2. [ ] Let's test those permission changes. Try to run the script:

```bash
./genids.sh
```

You should get an error like this:

```bash
permission denied
```

3. [ ] That error occurs because the executable permission has been removed. Let's re-add the executable permission for the owner:

```bash
chmod u+x <filename>
```

4. [ ] Once you've done that, try running the `./genids.sh` script again.

Paste the output of the script into the input field and submit your answer.

## Tips

You'll frequently download scripts from the internet to run on your machine, and often you'll need to make them executable before you can run them. I use `chmod +x` quite often as a developer.

The internet is a shady place. Only run verified scripts from trusted publishers and authors.