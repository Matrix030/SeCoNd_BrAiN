You might be used to nice graphical interfaces that allow you to search for text in files, usually with `ctrl+f` or `cmd+f`. But what about when you're working on a terminal?

As it turns out, once you're used to it, searching for text in files on a CLI can be much faster than using a GUI.

## The grep Command

The [grep command](https://www.digitalocean.com/community/tutorials/grep-command-in-linux-unix) allows you to search for text in files. It has a _ton_ of capability, and we'll only be scratching the surface of its true power.

### Basic Usage

The most basic use for `grep` is to search for a string in a file. For example, if we wanted to search for the word "hello" in the file `words.txt`, we could run:

```bash
grep "hello" words.txt
```

This will print out every line in `words.txt` that contains the word "hello". It's a case-sensitive search, so it will only match "hello", not "Hello" or "HELLO".

## Assignment

Applications often write their logs to files on disk. These logs can contain useful information about what the application is doing, and can also be used to debug problems. As a security auditor, you need to dig through these logs to find any evidence of suspicious activity.

Use the `grep` command to find any lines with the text "CRITICAL" (all caps) in the `worldbanc/private/logs/2024-01-10.log` file.

Paste the output of your `grep` command into the input field and submit it.

## Tip

The tab key is your friend! If you start typing the name of a file or directory and then press tab, your shell will try to autocomplete the name for you. If there are multiple possible completions, it will show you a list of them. I rarely type out full file names, I type the first few characters and then press tab.