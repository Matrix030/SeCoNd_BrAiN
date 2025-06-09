Like Python, Ruby, and PHP, JavaScript is a dynamically-typed (not statically-typed) language. Its variable types are only known at _runtime_ (yes, yes, we'll talk about TypeScript in another course). **Some of us _like_ being able to see the types of our variables in our editors, okay**?

Now, _unlike Python_, it's also [weakly-typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing), meaning it will automatically convert types when you do things like add a number to a string. This can lead to some unexpected behavior if you're not careful...

```javascript
let answerToLife = 42;
let answerToTheUniverse = "42";

// obviously JavaScript thinks that adding strings
// and numbers is totally sane and normal behavior
const answerToEverything = answerToLife + answerToTheUniverse;

console.log(answerToEverything);
// "4242"
```