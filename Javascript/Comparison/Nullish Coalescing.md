JavaScript is truly a language built for the web, and the web runs on... _uncertainty_.

It's kinda crazy how much production JavaScript exists solely to handle cases where a value might be `null` or `undefined`.

The [`nullish coalescing`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) operator `??` is a way to handle these cases in a more concise way.

```js
let myName = null;
console.log(myName ?? "Anonymous"); // "Anonymous"

myName = "Lane";
console.log(myName ?? "Anonymous"); // "Lane"
```

If the value on the left of `??` is `null` or `undefined`, the value on the right is returned. Otherwise, the value on the left is returned. **It's a way to set sane defaults for variables that might be empty**.