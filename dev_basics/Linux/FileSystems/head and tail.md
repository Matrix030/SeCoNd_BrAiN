Sometimes you don't want to print _everything_ in a file. Files can be really big after all.

## The head Command

The [head](https://www.ibm.com/docs/en/aix/7.3?topic=h-head-command) command prints the first `n` lines of a file, where `n` is a number you specify.

```bash
head -n 10 file1.txt
```

If you don't specify a number, it will default to `10`.

## The tail Command

The [tail](https://www.ibm.com/docs/en/aix/7.3?topic=t-tail-command) command prints the _last_ `n` lines of a file, where `n` is a number you specify.

```bash
tail -n 10 file1.txt
```

## Assignment

Time to start investigating the bank's transactions.

1. [ ] Use the `cd` command to get into the `worldbanc/private/transactions/` directory.
2. [ ] Run the `cat` command to view the contents of the `2023.csv` file. You'll notice that it's a _really_ long file. We don't want to print the whole thing.
3. [ ] Use the `head` command to print the _first_ 6 lines of the `2023.csv` file and copy them into the input field – **do not submit yet**

_We print the first 6 lines because the first line in the file is the header, and we want to include that along with the first 5 transactions._

4. [ ] Use the `tail` command to print the _last_ 5 lines of the `2023.csv` file and copy them into the input field

Submit the **combined first 6 and last 5 lines**, for a total of 11 lines.

## Tip

Remember, you can use `..` as an alias for a parent directory. So if you're in `worldbanc/private/transactions/` and you want to get to `worldbanc`, you can run `cd ..` twice:

```bash
cd ..
cd ..
```

Alternatively, you can just run:

```bash
cd ../..
```

once.