JavaScript supports [first-class](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function) and higher-order functions, which are just fancy ways of saying "functions as values". Functions can be treated like any other data type—such as `number`s and `string`s and `boolean`s. Let's assume we have two simple functions:

```javascript
function add(x, y) {
  return x + y;
}

function mul(x, y) {
  return x * y;
}
```

We can create a new `aggregate` function that accepts a function as its 4th argument:

```javascript
function aggregate(a, b, c, arithmetic) {
  const firstResult = arithmetic(a, b);
  const secondResult = arithmetic(firstResult, c);
  return secondResult;
}
```

It calls the given `arithmetic` function (which could be `add` or `mul`, or any other function that accepts two parameters and returns a number) and applies it to three inputs instead of two. It can be used like this:

```javascript
function main() {
  const sum = aggregate(2, 3, 4, add);
  // sum is 9
  const product = aggregate(2, 3, 4, mul);
  // product is 24
}
```