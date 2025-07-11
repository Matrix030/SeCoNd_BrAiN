HTTPS requires that the client use [SSL](https://developer.mozilla.org/en-US/docs/Glossary/SSL) or [TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS) to protect requests and traffic by encrypting the information in the request. You don’t need to manually handle encryption or decryption—it's all done for you by the protocol. **HTTPS is simply HTTP with built-in security!**

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/uw8x4XC-1280x539.png)

## HTTPS != Privacy

We won't cover _how encryption works_ in this course, but it's important to understand _what it does and doesn't do_, at least in regards to HTTPS.

- HTTPS will encrypt the data in a request (like passwords, headers, body information, query parameters, etc). You and the recipient are the only ones who can read that information.
- HTTPS will not hide _who you are_ or that you're communicating with the given server. If you don't want your wife who knows how to sniff network traffic to know you're using Boot.dev, HTTPS won't help.

## HTTPS Verifies the Server

In addition to encrypting the information within a request, HTTPS uses [digital signatures](https://en.wikipedia.org/wiki/Digital_signature) to prove that you're communicating with the server that you think you are. If a hacker were to intercept an HTTPS request by tapping into a network cable, they wouldn't be able to successfully pretend they are your bank's web server.