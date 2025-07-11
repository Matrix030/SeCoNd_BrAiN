You may be wondering:

> How the heck am I supposed to memorize how all these different servers work???

The good news is that _you don't need to_. When you work with a backend server, it's the responsibility of that server's developers to provide you with instructions, or _documentation_ that explains how to interact with it. For example, the documentation should tell you:

- The domain of the server
- The resources you can interact with (HTTP paths)
- The supported query parameters
- The supported HTTP methods
- Anything else you'll need to know to work with the server

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/GIlWhYF-500x553.jpg)

## The Server Is the Captain Now

The server has _complete control_ over how the path in a URL is interpreted and used in a request. The same goes for query parameters. While there are a lot of strong _conventions_ around how servers _should_ interpret paths and query parameters, the server can do whatever it wants. That's why you need docs.