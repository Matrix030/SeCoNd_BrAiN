## Janky Syntax

You _can_ write an `if` statement without braces if you only have one statement in the body:

```c
if (x > 3) printf("x is greater than 3\n");
```

Buuuuut this shorthand is easy to mess up and in my opinion isn't worth saving a couple lines. **There are enough ways to shoot yourself in the foot in C already.**
