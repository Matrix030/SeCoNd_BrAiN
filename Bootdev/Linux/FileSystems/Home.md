In a Unix-like operating system, a user's [home directory](https://en.wikipedia.org/wiki/Home_directory) is the directory where their personal files are stored. It is also the directory that a user starts in when logging into the system.

I recommend doing all of your development work in your home directory. For example, I like to create a `workspace` directory in my home directory, and all my programming projects live in subdirectories there.

## Danger

Your home directory is where you want to spend most of your time. Many of the other directories on your machine are critical to the operating system or other programs. Be careful when working in [other directories](https://www.ibm.com/docs/en/aix/7.3?topic=reference-directories) like `/bin`, `/etc`, `/var`, etc.

You can mess up your system if you're not careful.

## The `~` Alias

My home directory (on Mac) is located at `/Users/wagslane`. The `~` character is an alias for your home directory. So when I want to go home, I don't have to type out `cd /Users/wagslane`, I can just type:

```bash
cd ~
```