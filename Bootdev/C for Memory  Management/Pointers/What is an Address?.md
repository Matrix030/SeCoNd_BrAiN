So I mentioned in the last lesson that memory can be thought of as a big array of bytes, and each byte has an address.

That's true, and the beauty is that each address is _literally just a number_. It's not some _mortal_ address like "1234 Elm St." or "1600 Pennsylvania Ave." It's **just a number**.

You might be thinking, "Hey, if it's just a number, why does it look all disgusting like `0xfff8`?"

That's because `0xfff8` _is_ just a number. But:

1. It's written in [`hexadecimal`](https://www.wikipedia.org/wiki/Hexadecimal) (base 16) instead of decimal (base 10).
2. It's a pretty big number, so it's not very human readable. `0xfff8` is the same as `65,528` in decimal.

![memory is just a number](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/6cprHiB.png)