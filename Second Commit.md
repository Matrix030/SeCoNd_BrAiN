Reminder of command syntax:

- `git log` (`q` to exit, arrow keys to scroll)
- `git cat-file -p <hash>`

`log` is a porcelain command, while `cat-file` is a plumbing command. You'll use `log` much more often when working on coding projects, but `cat-file` is useful for us in this course to understand Git's internals.

## Assignment

1. [ ] Create a second file called `titles.md` and add the following text:

```md
# Titles

- A River Runs Through It
- Fight Club
- 12 Years a Slave
- The Big Short
- 12 Monkeys
```

2. [ ] Save, stage, and commit the file with any commit message you like as long as it starts with `B:`.
    - For example, `B: add titles`.
3. [ ] Use (`git cat-file`) to inspect the commit you just made. _You should notice one extra field in the commit object that wasn't present in the first commit._

**Run and submit** the CLI tests from the **root of your repo**.