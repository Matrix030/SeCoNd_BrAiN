The [man](https://www.ibm.com/docs/en/aix/7.3?topic=m-man-command) command is short for "manual". It's a program that displays the manual for other programs.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/tpIPc6I-958x500.jpg)

I know that manuals and documentation can feel intimidating, heck, that's why there are courses like this one to give you a gentler introduction. That said, manuals and documentation will become more useful to you as you become a more experienced developer. They're not as scary as they seem when you actually take the time to read them.

## Using man

The `man` command will only work for programs that it has a manual for, but most built-in commands and Unix programs are supported. You just pass the name of the command as an argument. The most intuitive place to start, of course, is reading the manual's manual:

```bash
# open the man pages for the 'man' command
man man
```

## Searching

Most people don't just [read man pages for fun](https://www.youtube.com/watch?v=rT-fbLFOCy0). More often, the manual is used as a reference to quickly look up usage or special flags.

You can search for text in the manual by pressing the `/` key and typing your search, then pressing enter. Let's try to look up what the `-r` flag does for the `ls` command:

```bash
man ls
# type '/-r' to start searching

# press 'n' to jump to the next result

# press 'N' to go back if you went too far
```