A common problem you'll run into in the future is that you install a new program on your machine, but when you try to run it from your terminal, you get an error like:

```bash
$ my-new-program
-bash: my-new-program: command not found
```

Nine times out of ten, it's because the program is installed in a directory that's not in your `PATH` variable. Oftentimes when you install a program using the CLI, it will print a message during the installation process that tells you where the command was installed. **Don't let your eyes glaze over when your terminal prints important messages!** Sometimes you just gotta [rtfm](https://www.dictionary.com/browse/rtfm).