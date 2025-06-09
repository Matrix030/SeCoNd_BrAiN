A ["truthy"](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) value is a value that is considered `true` when encountered in a Boolean context. In JavaScript, you don't _need_ to explicitly convert a value to a Boolean before using it in a conditional:

```javascript
if ("hello") {
  console.log("hello is truthy");
}
if (42) {
  console.log("42 is truthy");
}
// hello is truthy
// 42 is truthy
```

A ["falsy"](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) value works the same way, but for values that evaluate to `false`:

```javascript
if (0) {
  console.log("0 is falsy");
}
if (null) {
  console.log("null is falsy");
}
if (undefined) {
  console.log("undefined is falsy");
}
```

Common truthy values include:

- `true`
- `42` (any number that isn't `0`)
- `"hello"` (any non-empty string)
- `[]` (an empty array)
- `{}` (an empty object)
- `function() {}` (an empty function)

Common falsy values include:

- `false`
- `0`
- `""` (an empty string)
- `null`
- `undefined`
- `NaN` (Not a Number)