Instagram would be pretty terrible if you had to manually copy your photos to your friend's phone when you wanted to share them. Modern applications need to be able to communicate information _between devices_ over the internet.

- Gmail doesn't just store your emails in variables on your computer, it stores them on computers in their data centers
- You don't lose your Slack messages if you drop your computer in a lake, those messages exist on Slack's [servers](https://en.wikipedia.org/wiki/Web_server)

## How Does Web Communication Work?

When two computers communicate with each other, they need to use the same rules. An English speaker can't communicate verbally with a Japanese speaker, similarly, two computers need to speak the same language to communicate.

This "language" that computers use is called a [protocol](https://en.wikipedia.org/wiki/Communication_protocol). The most popular protocol for web communication is [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), which stands for Hypertext Transfer Protocol.

## Jello

Throughout this course, we'll be building parts of an online issue tracking app called "Jello". It's a typical issue tracker with one key difference: this time it's good, actually.