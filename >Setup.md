[Git](https://git-scm.com/) is _the_ distributed [version control system](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) (VCS). Nearly every developer in the world uses it to manage their code. It has quite a monopoly on VCS. Developers use Git to:

- Keep a history of their code changes
- Revert mistakes made in their code
- Collaborate with other developers
- Make backups of their code
- And much more

## Boot.dev CLI

Throughout this course, you'll be using the Boot.dev CLI to run our tests (which are just CLI commands) against your local environment. [Install it now](https://github.com/bootdotdev/bootdev?tab=readme-ov-file#installation) if you don't already have it. All the instructions and troubleshooting info are on the GitHub page.

Make sure the Boot.dev CLI install worked:

```bash
bootdev --version
```

_If you're stuck, reach out in the help forums of the [Discord](https://www.boot.dev/community)._

Once the `bootdev` command is working, log in and follow the instructions:

```bash
bootdev login
```

## Run vs. Submit

Lessons have a series of "commands" that run on your local machine, and a series of tests it will check against the results of the command. There are two ways to run these commands: `run` and `submit`:

1. `bootdev run <id>`: This will run the commands and show you the results. It's to be used for debugging, but it won't tell you whether or not you've passed the tests explicitly.
2. `bootdev run <id> -s`: This will run the commands and give you pass/fail feedback. It will also mark the lesson as complete on the website. If you get it wrong however, you'll potentially lose your sharpshooter spree, so be sure to use `run` first!

You can copy the run/submit commands with the id ready-to-go from the test panel.

## Assignment

Once you have the CLI installed and you're logged in, copy and paste the `run` command from the right into your terminal and execute it. If it's doing what you'd expect (printing `rebasing is based`), then run the submit command.

Boots

Spellbook

Community

![Boots](https://www.boot.dev/_nuxt/new_boots_profile.DriFHGho.webp)

**Need help?** I, Boots the Master of Code and Casting, can assist... _for a price_.

Copy/paste one of the following commands into your terminal:

Run

bootdev run 65e6780d-fdde-447a-9898-b30b73793a3a

Submit

bootdev run 65e6780d-fdde-447a-9898-b30b73793a3a -s

Run the CLI commands to test your solution.

1. echo "rebasing is based"
    - Expecting exit code: 0
    - Expecting stdout to contain all of:
        
        - rebasing is based
        

Using the Bootdev CLI

The Bootdev CLI is the only way to submit your solution for this type of lesson. We need to be able to run commands in your environment to verify your solution.

You can [install it here](https://github.com/bootdotdev/bootdev). It's a Go program hosted on GitHub, so you'll need Go installed as well. Instructions are on the GitHub page.