Let's add an "admin" namespace. This is where we'll put endpoints intended for internal administrative use. Note: there's nothing inherently more secure about this namespace, it's just an organizational structure.

## Assignment

1. [ ] Swap out the `GET /api/metrics` endpoint, which just returns plain text, for a `GET /admin/metrics` that returns HTML to be rendered in the browser. Use this template:

```html
<html>
  <body>
    <h1>Welcome, Chirpy Admin</h1>
    <p>Chirpy has been visited %d times!</p>
  </body>
</html>
```

Where `%d` is replaced with the number of times the page has been loaded.

- [ ] Make sure you use the `Content-Type` header to set the response type to `text/html` so that the browser knows how to render it.
- [ ] Try loading `http://localhost:8080/admin/metrics` in your browser, and in another tab load `http://localhost:8080/app` a few times. Refreshing the admin page should show the updated count.

2. [ ] Update the `POST /api/reset` to `POST /admin/reset`. It's functionality should not change.

**Run and submit** the CLI tests.