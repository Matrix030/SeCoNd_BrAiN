The `contents.md` file has been created, but as we saw, it's _untracked_. We need to stage it (add it to the "index") with the [git add](https://git-scm.com/docs/git-add) command before committing it later.

Without staging, no files are included in the commit—only the files you explicitly git add will be committed.

Here's the command:

`git add <path-to-file | pattern>`

For example:

```bash
git add i-use-arch.btw
```

## Assignment

1. [ ] Add `contents.md` to the staging area.
2. [ ] Run `git status` again.

**Run and submit** the CLI tests.