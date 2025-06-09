If you're coming from Python, you might be thinking:

> Ah, so `undefined` is like `None`! Easy peasy.

Yes. But also... **no**. One of JavaScript's most cursed features is that it has two values for "nothing":

- [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined): It doesn't exist _at all_. In grug-speak `undefined` is "very nothing"
- [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null): It (kind of) exists, but it's _empty_. In grug-speak `null` is "kinda nothing"

There are [some practical differences between the two](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null#difference_between_null_and_undefined), the primary one being that `undefined` is the _default_ value of a variable when it hasn't been given a value yet.

```js
let myName;
console.log(myName); // undefined
```

To get a `null` value, you have to explicitly assign it:

```js
let myName = null;
console.log(myName); // null
```

Confusingly, `typeof` returns `"object"` for `null`:

```js
console.log(typeof null); // object
```

_To be clear, `null` **is** its own type according to the [ECMAScript specification](https://tc39.es/ecma262/#sec-ecmascript-language-types), but the "object" type report is a historical quirk that [can't be easily fixed now](https://web.archive.org/web/20071020084354/http://wiki.ecmascript.org/doku.php?id=proposals%3Atypeof)_.

In _most cases_, `null` and `undefined` work the same way, but you'll want to be consistent in _how_ you use them, and know that there are subtle behavioral differences.