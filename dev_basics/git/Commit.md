After staging a file, we can [commit](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/about-commits) it.

A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.

Here's how to [commit](https://git-scm.com/docs/git-commit) all of your staged files:

```bash
git commit -m "your message here"
```

## Assignment

1. [ ] Commit the `contents.md` file with the message `A: add contents.md`. _You wouldn't normally start a commit message with "A:", you'd just write the message, but we're going to use letters to help us easily identify commits in this course._
2. [ ] Run `git status` again, and you should see that the file is no longer staged.

**Run and submit** the CLI tests from **inside the `webflyx` directory**.

## Tip

If you screw up a commit message, you can change it with the `--amend` flag. For example:

```bash
# Change the last commit message
git commit --amend -m "A: add contents.md"
```