[Switch statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) are a way to compare a variable against multiple possible values. They are similar to if-else statements, but tend to be more readable when there are many potential options.

```javascript
const os = "mac";
let creator;
switch (os) {
  case "linux":
    creator = "Linus Torvalds";
    break;
  case "windows":
    creator = "Bill Gates";
    break;
  case "mac":
    creator = "Steve";
    break;
  default:
    creator = "Unknown";
    break;
}

console.log(creator);
// Steve
```

Unlike some languages where [fall-through](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch#breaking_and_fall-through) doesn't happen by default, JavaScript **will continue** to execute the next case until it reaches a `break` or `return` statement.

_99 times out of 100, you'll want to include a `break/return` statement after each case to prevent this behavior_.