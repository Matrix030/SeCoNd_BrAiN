In JavaScript, a (non-template) string can be written with either single or double quotes. For example:

- `'Hello'`
- `"Hello"`.

**Personally, we prefer double quotes**! It's important to have styling conventions so that all the code in a project looks consistent, making it easier to read and contribute to.

## Indexing

Square brackets are used to access individual characters inside a string. The characters are numbered from `0` to `length-1`. It's similar to how strings and lists work in Python, Go and many other languages.

```js
const greeting = "Hello";
greeting[0]; // 'H'
greeting[1]; // 'e'
greeting[2]; // 'l'
greeting[3]; // 'l'
greeting[4]; // 'o'
// you can also get the last char at length-1
greeting[greeting.length - 1]; // 'o'
```

The [`.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length) property is used to get the number of characters in a string.