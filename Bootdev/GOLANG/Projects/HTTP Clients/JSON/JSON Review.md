JSON is a _stringified representation_ of a [JavaScript object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects), which makes it perfect for saving to a file or sending in an HTTP request. Remember, an actual JavaScript object is something that exists only within your program's variables. If we want to send an object outside our program, for example, across the internet in an HTTP request, we need to convert it to JSON first.

## JSON Isn't Just for JavaScript

Just because JSON is called _JavaScript_ Object Notation doesn't mean it's only used by JavaScript code! JSON is a common standard that is recognized and supported by every major programming language. For example, even though Boot.dev's backend is written in Go, we still use JSON as the communication format between the front-end and backend.

## Common Use-Cases

- In HTTP request and response bodies
- As formats for text files. `.json` files are often used as configuration files.
- In NoSQL databases like MongoDB, ElasticSearch and Firestore

## Pronouncing JSON

I pronounce it "Jay-sawn", but I've also heard people pronounce it "Jason" (like the name), and even "J'ai son" (Frenchily).