As you've already seen in this course, some commands accept [flags](https://www.ibm.com/docs/en/aix/7.3?topic=names-command-flags). Flags are options that you can pass to a command to change its behavior.

For example, the `ls` command can take a `-l` flag to show a "long" listing of files:

```bash
ls -l
```

Or the `-a` flag to show "all" files, including hidden files:

```bash
ls -a
```

You can also combine flags:

```bash
ls -al
```

## Conventions

Whether or not a command takes flags, and what those flags are, is up to the developer of the command. That said there are some common conventions:

- Single-character flags are prefixed with a single dash (.e.g `-a`)
- Multi-character flags are prefixed with two dashes (e.g. `--help`)
- Sometimes the same flag can be used with a single dash or two dashes (e.g. `-h` or `--help`)