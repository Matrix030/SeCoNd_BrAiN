As you might have guessed, JavaScript supports functions via the [`function` keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function).

Even JavaScript isn't crazy enough to not support functions.

```js
// function declaration
function getSum(a, b) {
  return a + b;
}

// function call
const result = getSum(60, 9);

console.log(result);
// 69
```