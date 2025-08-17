Luckily, Git has a built-in plumbing command, [cat-file](https://git-scm.com/docs/git-cat-file), that allows us to see the contents of a commit without needing to futz around with the object files directly.

```bash
git cat-file -p <hash>
```

## Assignment

1. [ ] Use the `cat-file` command to view the contents of your last commit. (Use `-p` for pretty-print.)
2. [ ] For the CLI to be able to check your output, do it again, but redirect the output to a temporary file:

```bash
catfile-command-here > /tmp/catfileout.txt
```

**Run and submit** the CLI tests.

## Tip

Use `git log -1` to get the hash of your last commit.