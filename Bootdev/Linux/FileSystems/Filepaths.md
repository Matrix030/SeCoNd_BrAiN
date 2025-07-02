The output of `pwd` is a _filepath_. A filepath is a string that describes the location of a file or directory on your computer. Yours should look _something_ like this:

```
/Users/wagslane
```

The text might be different, but the structure should be the same. Let's break it down:

- The first slash (`/`) represents the _root directory_. It's the tippy-top of the filesystem tree.
- The next part (`Users`) is the name of a directory inside the root directory.
- Finally, the last part (`wagslane`) is the name of a directory inside the `Users` directory.

So this path represents a directory 2 levels down from the root directory:

```
root
  └── Users
        └── wagslane
```

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: Linux filepath

## Assignment

Time to start digging for evidence. _If you're on Windows, again, make sure you're in the Ubuntu (WSL) terminal_.

1. [ ] Copy/paste and run the command below to download the `worldbanc` directory from GitHub. It contains files and directories that you'll need throughout this course. If prompted for a password, use the password for your machine's user account, or the one you used when setting up Ubuntu in WSL.

```bash
curl -L https://github.com/bootdotdev/worldbanc/archive/refs/heads/main.zip -o worldbanc.zip
unzip worldbanc.zip
rm worldbanc.zip
mv worldbanc-main worldbanc
sudo chown -R $(whoami) worldbanc
sudo chmod -R 755 worldbanc
```

We'll cover what these commands do later. If you run into an issue, see the **troubleshooting tips** below.

**You should now have a `worldbanc` directory in your current working directory**. When learning terminal commands in this course, it's possible to make a mistake and ruin your version of the worldbanc repo. If that happens, just come back to this lesson and download `worldbanc` again.

2. [ ] Run the "list" command to see the contents of your current working directory:

```bash
ls
```

You should see a `worldbanc` directory listed.

3. [ ] Run the "change directory" command to move into the `worldbanc` directory:

```bash
cd worldbanc
```

4. [ ] Use the `ls` command again to see the contents of the `worldbanc` directory. Paste the console output into the text field and submit your answer.

## Troubleshooting

If you're having issues with the download due to [`curl`](https://curl.se/docs/manpage.html) or [`unzip`](https://www.linux.org/docs/man1/unzip.html) not being installed on Ubuntu/WSL, you can install them with the following commands:

```bash
sudo apt install unzip
sudo apt install curl
```

Certain Windows versions may not let you paste into the Command Line. See [how to enable ctrl+shift+v](https://superuser.com/questions/1410026/how-to-enable-ctrl-shift-v-in-windows-subsystem-for-linux-wsl-command-pr).