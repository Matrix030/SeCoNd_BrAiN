We talked about how you can create and use local variables in your shell:

```bash
name="Lane"
echo $name
# Lane
```

There is another type of variable called an [environment variable](https://en.wikipedia.org/wiki/Environment_variable). They are available to _all_ programs that you run in your shell.

You can view all of the environment variables that are currently set in your shell with the `env` command.

To set a variable in your shell, use the `export` command:

```bash
export NAME="Lane"
```

You can then use the variable in your shell, just as before:

```bash
echo $NAME
# Lane
```

The interesting part is that programs and scripts you run in your shell can _also_ use that variable:

For example, we can create a file called `introduce.sh` with the following contents:

```bash
#!/bin/sh
echo "Hi I'm $NAME"
```

Then we run it:

```bash
chmod +x ./introduce.sh

./introduce.sh
# Hi I'm Lane
```