`if` statements in JavaScript use parentheses around the condition:

```javascript
if (height > 4) {
  console.log("You are tall enough!");
}
```

`else if` and `else` are supported as you might expect:

```javascript
if (height > 6) {
  console.log("You are super tall!");
} else if (height > 4) {
  console.log("You are tall enough!");
} else {
  console.log("You are not tall enough!");
}
```

We'll cover some more of these, but for now, here are some common comparison operators:

- `===` equal to
- `!==` not equal to
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to