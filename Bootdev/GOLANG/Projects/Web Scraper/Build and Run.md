Let's just get our Go project set up and running. We'll start with a simple "Hello, World!" program.

## Assignment

1. [ ] Create a new `main.go` file in your project root. It should be part of `package main`. Add a `main` function that simply prints `"Hello, World!"` to the console.
2. [ ] [Create a Go module](https://golang.org/doc/tutorial/create-module) in the root of your project. Here's the command:

```bash
go mod init MODULE_NAME
```

I recommend naming the module by its remote Git location (you should store all your projects in Git!). For example, my GitHub name is `wagslane` so my module name might be `github.com/wagslane/crawler`.

3. [ ] Build your program:

```bash
go build -o crawler
```

4. [ ] Run your program:

```bash
./crawler
```

You should see `"Hello, World!"` printed to the console.

5. [ ] Add a [.gitignore](https://git-scm.com/docs/gitignore) file to your project root and have Git ignore the `crawler` (your module name) binary file.

**Run and submit** the CLI tests from **your project's root**.