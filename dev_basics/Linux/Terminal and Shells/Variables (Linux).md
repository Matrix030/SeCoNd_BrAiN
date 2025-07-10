- If you're using Ubuntu on WSL, you're probably running a [Bash](https://en.wikipedia.org/wiki/Bash_\(Unix_shell\)) shell.
- If you're using macOS, you're probably running a [Zsh](https://en.wikipedia.org/wiki/Z_shell) shell.
- If you're running full Linux, I pray you already know what you're using.

The point is that you're probably using Bash or Zsh, and for the purposes of this course, they're _basically_ the same.

Both Bash and Zsh are shells, and they also happen to be powerful programming languages. They have variables, functions, loops, and more. That said, only [crazy people](https://bashsta.cc/) write large programs in shell languages... shells are optimized for running other programs and writing small scripts, not for writing large applications.

## Create a Variable

```bash
name="Lane"
```

Notice there are no spaces between the variable `name`, assignment operator `=` and value `"Lane"`.

## Use a Variable

```bash
$ echo $name
Lane
```

Unlike in Python, where you can just use a variable's _name_, in your shell you need to prefix the variable name with a `$` to _use_ it, else it would just print `name` instead of the value.

## Interpolate a Variable in a String

```bash
$ echo Hello $name
Hello Lane
```

It's really just _that_ easy.