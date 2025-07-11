The HTTP [`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) method creates _or more commonly, updates_ the target resource with the contents of the request's `body`. Unlike `GET` and `POST`, there is no `http.Put` function. You will have to create a raw `http.Request` that an `http.Client` can [`Do`](https://pkg.go.dev/net/http#Client.Do).

## POST vs. PUT

While `POST` and `PUT` are both used for creating resources, `PUT` is more common for updates and is [idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent), meaning it's safe to make the same request multiple times without changing the server state more than once. For example:

```
POST /users/bob (create bob user)
POST /users/bob (create duplicate bob user)
POST /users/bob (create duplicate bob user)
```

```
PUT /users/bob (create bob user if it doesn't exist)
PUT /users/bob (update bob user with the same data)
PUT /users/bob (update bob user with the same data)
```