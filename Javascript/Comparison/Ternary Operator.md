Sometimes using 3-5 lines of code to write an if/else block is overkill. The [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) makes it easy to write a conditional as a single [expression](https://en.wikipedia.org/wiki/Expression_\(computer_science\)).

```js
const price = isMember ? "$2.00" : "$10.00";
```

I like to read it in English as:

> If `isMember` is true, evaluate to `$2.00`, otherwise evaluate to `$10.00`.

The same logic using if/else would be:

```js
let price;
if (isMember) {
  price = "$2.00";
} else {
  price = "$10.00";
}
```

## Why Is It Called a “Ternary”?

Ternary's latin root means "3", and it's the only JavaScript operator that takes _three_ operands.

- A condition followed by a question mark (?)
- An expression to execute if the condition is truthy followed by a colon (`:`)
- The expression to execute if the condition is falsy.