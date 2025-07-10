[JSON](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) (JavaScript Object Notation), is a standard for representing _structured_ data based on JavaScript's object syntax. It is commonly used to transmit data in web apps via HTTP. For example, The HTTP requests we have been making in this course have been returning _Jello_ issues as JSON.

JSON supports the following primitive data types:

- Strings, e.g. `"Hello, World!"`
- Numbers, e.g. `42` or `3.14`
- Booleans, e.g. `true`
- Null, e.g. `null`

And the following collection types:

- Arrays, e.g. `[1, 2, 3]`
- Object literals, e.g. `{"key": "value"}`

JSON is similar to JavaScript objects and Python dictionaries. Keys are always strings, and the values can be any data type, including other objects.

The following is valid JSON data:

```js
{
    "movies": [
        {
            "id": 1,
            "title": "Iron Man",
            "director": "Jon Favreau",
            "favorite": true
        },
        {
            "id": 2,
            "title": "The Avengers",
            "director": "Joss Whedon",
            "favorite": false
        }
    ]
}
```