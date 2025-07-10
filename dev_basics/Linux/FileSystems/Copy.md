The [copy command](https://www.ibm.com/docs/en/aix/7.3?topic=c-cp-command) does what you would (hopefully) expect: it copies a file from one location to another.

```bash
cp source_file.txt destination/
```

You can also copy a directory and all of its contents recursively by adding the `-R` flag:

```bash
cp -R my_dir new_dir
```

## Assignment

1. [ ] Take a look inside the `worldbanc/private/transactions/` directory. You should see a few files containing transactions from different years.
2. [ ] Look inside the `backups` directory inside of `transactions`. It looks like something is missing!
3. [ ] Copy the missing file from the `transactions` directory into the `backups` directory so there is a backup of all of the transactions.

List the contents of the `backups` directory and paste the output **without the command** into the input field and submit it.