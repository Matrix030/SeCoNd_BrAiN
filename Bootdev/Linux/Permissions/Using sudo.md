Before you start this assignment, make sure the permissions on the `worldbanc/private/contacts` directory are `drwx------`, that the owner is `root`, and that you're not signed in as root. If you've been following along, all of those should be true. You can check with the `ls -l` command.

## Assignment

You told senior staff about the `contacts` directory and its contents, and they've decided to delete it entirely.

1. [ ] Go ahead and _try_ to delete the `contacts` directory with the `rm` command:

```bash
rm -r contacts
```

The `contacts` directory should fail to delete! You do not have permission to delete it. It's owned by the `root` user, and you're not `root`.

2. [ ] Try again as root by using `sudo`.
3. [ ] Once you have confirmed that you were able to delete it, run the `ls` command from inside the `private` directory.

Paste the output of the `private` directory's contents into the input field and submit your answer.