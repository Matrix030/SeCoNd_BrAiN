On static sites (like blogs or documentation sites) a URL's path mirrors the server's filesystem hierarchy.

For example, if the website `https://exampleblog.com` had a static web server running in its `/home` directory, then a request to `https://exampleblog.com/site/index.html` would probably return the file located at `/home/site/index.html`.

_But technically, this is just a convention. The server could be configured to return any file or data given that path._

## It's Not Always That Simple

Paths in URLs are essentially just another type of parameter that can be passed to the server when making a request. For dynamic sites and web applications, the path is often used to denote a specific resource or endpoint.