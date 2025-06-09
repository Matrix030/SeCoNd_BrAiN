You should already be familiar with these inequality operators, and they work as you would expect in JavaScript:

```javascript
5 < 6; // evaluates to true
5 > 6; // evaluates to false
5 >= 6; // evaluates to false
5 <= 6; // evaluates to true
```

The [equality operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) however are a bit... _strange_. To compare two values to see if they are _exactly_ the same, use the [strict equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) (`===`) and [strict inequality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_inequality) (`!==`) operators:

```js
5 === 6; // evaluates to false
5 !== 6; // evaluates to true
```

The "normal" [equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) (`==`) and [inequality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Inequality) (`!=`) operators are a bit more... _flexible_:

```js
5 == 6; // evaluates to false
5 == "5"; // evaluates to true

5 != 6; // evaluates to true
5 != "5"; // evaluates to false
```

The "strict equals" (`===`) and "strict not equals" (`!==`) compare both the value _and_ the type. The "loose equals" (`==`) and "loose not equals" (`!=`) attempt to convert and compare values of different types. With the loose versions, the string `'5'` and the number `5` are considered "equal", which, in _good_ code, is usually _not_ what you want.

**This is a fairly unique quirk of the JavaScript language**.

You can [read more about how `==` works](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality#description) if you're interested, but I'd recommend sticking with `===` and `!==` in nearly all cases.