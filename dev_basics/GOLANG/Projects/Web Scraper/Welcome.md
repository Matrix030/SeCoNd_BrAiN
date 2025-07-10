In this project, we'll build a [Web Crawler](https://www.cloudflare.com/learning/bots/what-is-a-web-crawler/) in Golang! To rank well in Google Search, websites need to [internally link pages one to another](https://developers.google.com/search/blog/2008/10/importance-of-link-architecture). For example, a blog post about the _benefits_ of haircuts should probably link to my post about the best places to _get_ haircuts.

We're going to write a Golang [CLI](https://en.wikipedia.org/wiki/Command-line_interface) application that generates an "internal links" report for any website on the internet by crawling each page of the site.

## Learning Goals

- Get hands-on practice with local Go development and tooling
- Practice making HTTP requests in Go
- Learn how to parse HTML with Golang
- Practice unit testing

## Setup

Before we dive into the project, let's make sure you are all set up properly. You will need:

1. All instructions will assume a bash-like/unix-like environment. I recommend [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install) if you're on Windows so that you can... not be.
2. The latest [Go toolchain](https://go.dev/doc/install) installed.
3. If you're in VS Code, I recommend installing the [Go extension](https://marketplace.visualstudio.com/items?itemName=golang.Go). It's not required, but it makes working with Go a lot easier.
4. You will need the [Boot.dev CLI](https://github.com/bootdotdev/bootdev#bootdev-cli) installed, and you'll need to be [logged in](https://github.com/bootdotdev/bootdev?tab=readme-ov-file#usage).

[[Build and Run]]
[[Normalize URLs]]

