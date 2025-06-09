In JavaScript, [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) are a fantastic way to interpolate dynamic values into a string. They're JavaScript's version of Python's f-strings. For example:

```javascript
const shadeOfRed = 101;
console.log(`The shade is ${shadeOfRed}`);
// The shade is 101
```

Template literals _must start and end with a backtick_, and anything inside of the dollar-sign bracket enclosure is automatically _cast_ to a string.

custom strings in .log