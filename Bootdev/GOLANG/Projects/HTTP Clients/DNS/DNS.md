A ["domain name"](https://en.wikipedia.org/wiki/Domain_name) or ["hostname"](https://en.wikipedia.org/wiki/Hostname) is just one portion of a URL. We'll get to the other parts of a URL later.

For example, the URL `https://homestarrunner.com/toons` has a hostname of `homestarrunner.com`. The `https://` and `/toons` portions aren't part of the `domain name -> IP address` mapping that we've been talking about.

## The net/url Package

The `net/url` package is part of Go's standard library. You can instantiate a [URL struct](https://pkg.go.dev/net/url#Parse) using `url.Parse`:

```go
parsedURL, err := url.Parse("https://homestarrunner.com/toons")
if err != nil {
	fmt.Println("error parsing url:", err)
	return
}
```

And then you can [extract just the hostname](https://pkg.go.dev/net/url#URL.Hostname):

```go
parsedURL.Hostname()
// homestarrunner.com
```