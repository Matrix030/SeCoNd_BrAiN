The primitive [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) value represents the absence of a value... but it's not the same thing as an "undeclared" variable!

We can create an un_defined_ variable by giving it a name, but no value:

```js
let favoriteSandersonCharacter; // undefined
console.log(typeof favoriteSandersonCharacter); // "undefined"
```

However if we _never create the name_, it's "un_declared_", and undeclared variables actually throw an error (so don't do this):

```js
console.log(favoriteRothfussCharacter); // ReferenceError: favoriteRothfussCharacter is not defined
```

The worst part is that the undeclared variable error actually says "not defined"... welcome to JavaScript. 🤦‍♂️

As an aside, you can use `const` to declare a variable that is assigned to `undefined`, but you wouldn't be able to set its value later, so why would you want to?

```js
const favoriteSandersonCharacter; // SyntaxError: missing = in const declaration
const favoriteSandersonCharacter = undefined; // undefined
```