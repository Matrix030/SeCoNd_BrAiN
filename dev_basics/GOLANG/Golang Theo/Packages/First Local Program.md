1. [ ] Once inside your personal workspace, create a new directory and enter it:

```bash
mkdir hellogo
cd hellogo
```

2. [ ] Inside the directory declare your module's name:

```bash
go mod init {REMOTE}/{USERNAME}/hellogo
```

Where `{REMOTE}` is your preferred remote source provider (i.e. `github.com`) and `{USERNAME}` is your Git username. If you don't use a remote provider yet, just use `example.com/username/hellogo`

3. [ ] Print your `go.mod` file:

```bash
cat go.mod
```

**Run and submit** the CLI tests using the [Boot.dev CLI](https://github.com/bootdotdev/bootdev) from the **root of the `hellogo` package**.