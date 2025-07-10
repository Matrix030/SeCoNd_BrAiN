The [more](https://www.ibm.com/docs/en/aix/7.3?topic=m-more-command) and [less](https://man7.org/linux/man-pages/man1/less.1.html) commands let you view the contents of a file, one page (or line) at a time.

As the adage goes, `less` is `more`.

In the context of these commands, `less` is _literally_ `more`. The `less` command does everything that the `more` command does but also has more features. As a general rule, you should use `less` instead of `more`.

You would only use `more` if you're on a system that doesn't have `less` installed.

## Assignment

You found nothing suspicious in the first and last transactions of 2023, but you're not done yet! It's time to dig through the middle of the file as well.

1. [ ] Run `less` and pass in the path to the `2023.csv` file, located at the root in the `worldbanc/private/transactions` directory.

```bash
less 2023.csv
```

Notice that you're now in an interactive mode and you've lost your shell prompt! That's because `less` has taken over your terminal window.

2. [ ] Press "enter" a few times to scroll down a few lines, just to see how that works. Press "q" to exit the `less` program and return to your shell prompt.
3. [ ] Re-run the `less` command, but this time, pass in the `-N` flag to show line numbers:

```bash
less -N 2023.csv
```

You can use the spacebar to scroll down a page at a time, and you can go back up by pressing "b".

4. [ ] Find line `153`. Copy and paste the contents of that line into the text field and submit it.

You can use "q" to exit `less` at any time.