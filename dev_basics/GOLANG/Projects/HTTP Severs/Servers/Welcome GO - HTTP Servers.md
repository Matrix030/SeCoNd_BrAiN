In a way, this course is where _everything_ you've learned so far on the Boot.dev back-end track comes together. Building HTTP servers is the bread-and-butter of a backend developer's day-to-day work.

This course assumes you already have a solid understanding of Go. If you don't, take a step back and take our [Go course](https://www.boot.dev/courses/learn-golang).

## Goals of This Course

- Understand what web servers are and how they power real-world web applications
- Build a production-style HTTP server in Go, without the use of a framework
- Use JSON, headers, and status codes to communicate with clients via a RESTful API
- Learn what makes Go a great language for building fast web servers
- Use type safe SQL to store and retrieve data from a Postgres database
- Implement a secure authentication/authorization system with well-tested cryptography libraries
- Build and understand webhooks and API keys
- Document the REST API with markdown

## What Is a Server?

A web [server](https://en.wikipedia.org/wiki/Server_%28computing%29) is just a computer that serves data over a network, typically the Internet. Servers run software that listens for incoming requests from clients. When a request is received, the server responds with the requested data.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nUzP2kg-1280x720.png)

Any server worth its salt can handle _many_ requests at the same time. In Go, we use a new [goroutine](https://go.dev/tour/concurrency) for each request to handle them concurrently. Let's start by practicing with goroutines.

## Assignment

In this course, we'll be working on a product called "Chirpy". Chirpy is a social network similar to Twitter.

One of Chirpy's servers is processing requests _unbelievably_ slowly. Use a goroutine to fix the bug in the `handleRequests` (not `handleRequest`) function. The server should be able to process all the requests within the time limit.