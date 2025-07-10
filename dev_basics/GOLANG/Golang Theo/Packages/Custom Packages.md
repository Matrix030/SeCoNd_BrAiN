Let's write a package to import and use in `hellogo`.

## Assignment

1. [ ] Create a _sibling_ directory in the parent directory of the `hellogo` directory:

```bash
cd ..
mkdir mystrings
cd mystrings
```

2. [ ] Initialize a module:

```bash
go mod init {REMOTE}/{USERNAME}/mystrings
```

3. [ ] Create a new file `mystrings.go` in that directory and paste the following code:

```go
// by convention, we name our package the same as the directory
package mystrings

// Reverse reverses a string left to right
// Notice that we need to capitalize the first letter of the function
// If we don't then we won't be able to access this function outside of the
// mystrings package
func Reverse(s string) string {
  result := ""
  for _, v := range s {
    result = string(v) + result
  }
  return result
}
```

Only capitalized names are exported, meaning they can be accessed by other packages. Uncapitalized names are private.

4. [ ] Notice there is no `main.go` or `func main()` in this package.
5. [ ] Run `go build` . Because this isn't a `main` package, it won't build an executable. However, `go build` will still compile the package and save it to our local build cache. It's useful for checking for compile errors.

**Run and submit** the CLI tests from the **root of the `mystrings` package**.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/08X3lqW-219x240.png)

Let's use our new `mystrings` package in `hellogo`.

## Assignment

1. [ ] Modify hellogo's `main.go` file with the below code. Don't forget to replace `{REMOTE}` and `{USERNAME}` with the values you used before.

```go
package main

import (
	"fmt"

	"{REMOTE}/{USERNAME}/mystrings"
)

func main() {
	fmt.Println(mystrings.Reverse("hello world"))
}
```

2. [ ] Edit hellogo's `go.mod` file to contain the following to import the local version of the `mystrings` dependency:

```gomod
module example.com/username/hellogo

go 1.23.0

replace example.com/username/mystrings v0.0.0 => ../mystrings

require (
	example.com/username/mystrings v0.0.0
)
```

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/08X3lqW-219x240.png)

`../mystrings` tells `go` to look in the parent directory of `hellogo` for the `mystrings` sibling directory.

3. [ ] Build and run the new program:

```bash
go build
./hellogo
```

**Run and submit** the CLI tests from the **root of the `main` package**.