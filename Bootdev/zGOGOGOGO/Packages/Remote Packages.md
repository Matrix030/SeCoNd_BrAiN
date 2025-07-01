Let's learn how to import and use an open-source package that's available on GitHub.

## A Note on “replace”

Be aware that using the "replace" keyword like we did in the last assignment _is not advised_, but can be useful to get up and running quickly. The _proper_ way to create and depend on modules is to publish them to a remote repository. When you do that, the "replace" keyword can be dropped from the `go.mod`:

This only works for local-only development:

```go
replace github.com/wagslane/mystrings v0.0.0 => ../mystrings
```

If we want the import to work for everyone, we need to make sure the dependency (`mystrings` in this case) actually exists on `https://github.com/wagslane/mystrings`.

## Assignment

1. [ ] Create a new directory in the same parent directory as `hellogo` and `mystrings` called `datetest`.
2. [ ] Create `main.go` in `datetest` and add the following code:

```go
package main

import (
	"fmt"
	"time"

	tinytime "github.com/wagslane/go-tinytime"
)

func main() {
	tt := tinytime.New(1585750374)
	tt = tt.Add(time.Hour * 48)
	fmt.Println("1585750374 converted to a tinytime is:", tt)
}
```

3. [ ] Initialize a module:

```bash
go mod init {REMOTE}/{USERNAME}/datetest
```

4. [ ] Download and install the remote [go-tinytime](https://github.com/wagslane/go-tinytime) package using `go get`:

```bash
go get github.com/wagslane/go-tinytime
```

5. [ ] Print the contents of your `go.mod` file to see the changes:

```bash
cat go.mod
```

6. [ ] Compile and run your program:

```bash
go build
./datetest
```

**Run and submit** the CLI tests from the **root of the `datetest` package**.