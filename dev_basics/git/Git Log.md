A Git repo is a (potentially very long) list of commits, where each commit represents the _full state of the repository_ at a given point in time.

The [git log](https://git-scm.com/docs/git-log) command shows a history of the commits in a repository. This is what makes Git a version control system. You can see:

- Who made a commit
- When the commit was made
- What was changed

## A Commit Hash

Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely identifies the commit. Here's an example of mine:

```
5ba786fcc93e8092831c01e71444b9baa2228a4f
```

For convenience, you can refer to any commit or change within Git by using the first `7` characters of its hash. For mine, that's `5ba786f`.

## Assignment

Run the `git log` command. You should see your commit. Notice that `git log` (assuming the log is long enough) starts an interactive pager. You can scroll through the log with the arrow keys, and exit by pressing `q`.

Next, run `git log` again, but this time use the `-n` and `--no-pager` options to limit the maximum number of commits shown, and more importantly, to run it without the interactive pager. E.g.:

```bash
git --no-pager log -n 10
```

**Run and submit** the CLI tests.