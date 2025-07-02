The "root" user is a superuser. It has access to everything on the system and can do anything. When you use the `sudo` command, you're running as the root user (as long as your system hasn't been configured differently).

The `sudo` keyword is convenient because it quickly gives you elevated permissions to run a single command.

![](https://imgs.xkcd.com/comics/sandwich.png)

-- [xkcd](https://xkcd.com/149/)

However, it can also be dangerous because it gives you access to everything. If you run a command with `sudo` that you don't understand, you could do serious damage to your system.

For example, `rm` with the `r` and `f` flags run on the root directory (`/`), will **delete all the files on your system**. Don't do that. The `r` flag is for "recursive" (delete everything inside) and the `f` flag is for "force". Most systems will prevent you from doing this, but if you run it with `sudo`, you've just turned your computer into a very expensive paperweight.

Some modern systems will actually prevent you from deleting everything by default as a safeguard unless you use `--no-preserve-root`, but it's still a very bad idea.

## Should I Use sudo?

Sure, as long as you understand what the command you're running does. **Just be careful**.