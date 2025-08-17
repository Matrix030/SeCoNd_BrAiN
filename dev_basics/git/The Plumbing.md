So far we've been using Git in a "porcelain" manner. But to sate our insatiable curiosity, let's take a look at some of the "plumbing".

## It's Just Files All the Way Down

All the data in a Git repository is stored directly in the (hidden) `.git` directory. That includes all the commits, branches, tags, and other objects we'll learn about later.

Git is made up of [objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) that are stored in the `.git/objects` directory. A commit is just a type of object.

## Assignment

1. Use `git log -n 10` to find your commit hash again.
2. Use `ls -al` to list the contents of the `.git/objects` directory.
3. Look for a directory that suspiciously matches the first two characters of your commit hash.
4. Use `ls -al` to list the contents of that directory and answer the question.