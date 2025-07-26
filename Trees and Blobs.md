Now that we understand some of our plumbing equipment, let's get into the pipes. Here are some terms to know:

- `tree`: git's way of storing a directory
- `blob`: git's way of storing a file

Here's what I got when I inspected my last commit:

```bash
> git cat-file -p 5ba786fcc93e8092831c01e71444b9baa2228a4f

tree 4e507fdc6d9044ccd8a4a3061324c9f711c4667d
author ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700
committer ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700

A: add contents.md
```

Notice that we can see:

- The `tree` object
- The `author`
- The `committer`
- The commit message

However, we _cannot_ see the contents of the `contents.md` file itself! That's because the `blob` object stores it.

## Assignment

1. [ ] Use `git cat-file -p` again, but this time with the hash of the `tree` object instead of the commit hash. You should see a `blob` object with _its_ own hash.
2. [ ] Use `cat-file` _again_ to view the contents of the `blob` object.
3. Run that same command, but this time [redirect the output](https://tldp.org/LDP/intro-linux/html/sect_05_01.html) to a temporary file: `/tmp/blobfile.txt`.

**Run and submit** the CLI tests.