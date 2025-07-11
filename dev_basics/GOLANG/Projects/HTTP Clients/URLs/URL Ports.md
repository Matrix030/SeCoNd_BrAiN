The port in a URL is a virtual point where network connections are made. Ports are managed by a computer's operating system and are numbered from `0` to `65,535` _(Though port `0` is reserved for the system API)_.

Whenever you connect to another computer over a network, you're connecting to a specific port on that computer, which is listened to by a program on that computer. A port can only be used by one program at a time, which is why there are so many possible ports.

The port component of a URL is often not visible when browsing normal sites on the internet, because 99% of the time you're using the default ports for the HTTP and HTTPS schemes: `80` and `443` respectively.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/gOCIIe4-632x283.png)

Whenever you aren't using a default port, you need to specify it in the URL. For example, port `8080` is often used by web developers when they're running their server in "test mode" on their own machines.