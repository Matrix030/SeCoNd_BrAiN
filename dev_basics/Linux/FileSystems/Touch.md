The [`touch`](https://man7.org/linux/man-pages/man1/touch.1.html) command updates the access and modification timestamps of a file. By default, if the specified file does not exist, `touch` will create an empty file with the given filename. Because of this side-effect, you’ll often see this command used to quickly create new empty files.

```bash
touch new_file.txt
```

You can also create multiple files at once by listing them:

```bash
touch some_file.txt some_other_file.txt
```

`touch` can be very handy when writing scripts because it ensures files exist without altering existing ones, creating new files only if necessary.

## Assignment

You discovered a discrepancy with the credit card files. WorldBanc is supposed to be keeping credit history records, but they aren't there!

1. [ ] Change into the `worldbanc/public/products/credit_cards` directory
2. [ ] Create a new file named `credithistory.txt` so they can keep track of that information
3. [ ] Use `ls` to verify that the file has been created successfully.

Paste the contents of the `credit_cards` directory into the text field and submit it.