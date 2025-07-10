The [`go run` command](https://pkg.go.dev/cmd/go#hdr-Compile_and_run_Go_program) quickly compiles and runs a Go package. The compiled binary is _not_ saved in your working directory.

I typically use `go run` to do local testing, scripting and debugging.

## Assignment

1. [ ] Inside `hellogo`, create a new file called `main.go`.

Conventionally, the file in the `main` package that contains the `main()` function is called `main.go`.

2. [ ] Paste the following code into your file:

```go
package main

import "fmt"

func main() {
	fmt.Println("hello world")
}
```

3. [ ] Run the code

```bash
go run main.go
```

**Run and submit** the CLI tests from the **root of the `main` package**.

## Tips

_You can execute `go help run` in your shell to rtfm._