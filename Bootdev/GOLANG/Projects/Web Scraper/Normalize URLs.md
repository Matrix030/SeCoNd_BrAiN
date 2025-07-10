[Test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) is a popular method of writing software. The idea is that you write tests for your code _first_, then you write the code that gets the tests to pass. We're going to approach this project using _a bit of_ TDD.

## Normalizing URLs

We need a function that accepts a URL string as input and returns a "normalized" URL. To "normalize" means to "make the same". For example, all of these URLs are the "same page" according to most websites and HTTP standards:

- `https://blog.boot.dev/path/`
- `https://blog.boot.dev/path`
- `http://blog.boot.dev/path/`
- `http://blog.boot.dev/path`

We want our `normalizeURL()` function to map all of those same inputs to a single normalized output: `blog.boot.dev/path`.

_Keep in mind the normalized URL isn't going to be used to make requests, it's just going to be used to compare URLs to see if they are the same page._

## Stub the Function

In test-driven development (TDD), we typically follow this flow:

1. Stub out the function
2. Write the tests
3. Write the code that passes the tests

If you'd like, you can read up on the standard library's [testing](https://pkg.go.dev/testing#pkg-overview) package and this [example of a unit test](https://go.dev/doc/tutorial/add-a-test) before you start.

## Assignment

1. [ ] Create a file called `normalize_url.go` and stub a function called `normalizeURL`.
2. [ ] Create a file called `normalize_url_test.go` in the root of your project.

The `go test` command will looks for and runs functions that end in `_test.go`,

3. [ ] Write a few tests! Remember, this function should take a single URL (string) and return the normalized version of the URL as a string. This will help us later to detect whether two different URLs point to the same _web page_, which is all our users care about. Here's a single [example unit test](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests) to get you started:

```go
func TestNormalizeURL(t *testing.T) {
	tests := []struct {
		name          string
		inputURL      string
		expected      string
	}{
		{
			name:     "remove scheme",
			inputURL: "https://blog.boot.dev/path",
			expected: "blog.boot.dev/path",
		},
        // add more test cases here
	}

	for i, tc := range tests {
		t.Run(tc.name, func(t *testing.T) {
			actual, err := normalizeURL(tc.inputURL)
			if err != nil {
				t.Errorf("Test %v - '%s' FAIL: unexpected error: %v", i, tc.name, err)
				return
			}
			if actual != tc.expected {
				t.Errorf("Test %v - %s FAIL: expected URL: %v, actual: %v", i, tc.name, tc.expected, actual)
			}
		})
	}
}
```

Try to test all the [edge-cases](https://en.wikipedia.org/wiki/Edge_case) you can think of.

4. [ ] Run your tests. `go test` will run all the tests in the current directory (package). `go test ./...` will run all the tests in the current directory and all subdirectories.

You should see some _failing tests_ now, which is expected because you haven't filled in the `normalizeURL` function yet.

5. [ ] Write the `normalizeURL` function. I recommend using the [Parse](https://pkg.go.dev/net/url#Parse) function from the `net/url` package. It parses a raw URL string into a URL struct with some fields and methods that will be useful to you.

**Run and submit** the CLI tests from the **root of the module**.