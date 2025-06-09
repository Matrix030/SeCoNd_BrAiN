In Python we use the `+=` operator to increment a number. That operator works in JavaScript as well, but JS also has a [`++` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Increment) for when you only want to increment by `1`.

```js
let bootdevCourseRating = 4;
bootdevCourseRating++;
console.log(bootdevCourseRating); // 5
bootdevCourseRating += 5;
console.log(bootdevCourseRating); // 10
```

And of course, there is a [`--` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Decrement) for decrementing by `1`.

```js
let bootdevCourseRating = 11;
bootdevCourseRating--;
console.log(bootdevCourseRating); // 10
bootdevCourseRating -= 5;
console.log(bootdevCourseRating); // 5
```