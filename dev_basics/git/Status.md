A file can be in one of [several states](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#_the_very_basics) in a Git repository. Here are a few important ones:

- `untracked`: Not being tracked by Git
- `staged`: Marked for inclusion in the next commit
- `committed`: Saved to the repository's history

The `git status` command shows you the current state of your repo. It will tell you which files are untracked, staged, and committed.

## Assignment

Every good content management system needs a table of contents.

1. [ ] Create a file in the root of your `webflyx` directory called `contents.md` and add the following text to the file:

```markdown
# contents
```

2. [ ] Save the file, then run:

```bash
git status
```

**Run and submit** the CLI tests from within the **`webflyx` directory**.

## Troubleshooting

This course expects Git's output to be in English. If it is not, you can temporarily set your terminal to English.

```sh
export LANG=en_US.UTF-8
```

You might need to install the [language pack](https://askubuntu.com/questions/149876/how-can-i-install-one-language-by-command-line) for your system, e.g. `sudo apt-get install language-pack-en` for Ubuntu.