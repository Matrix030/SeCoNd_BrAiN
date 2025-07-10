So we've talked about domain names, but we haven't talked about _the system_ that makes them work.

[DNS](https://en.wikipedia.org/wiki/Domain_Name_System), or the "Domain Name System", is the phonebook of the internet. Humans type easy-to-read [domain names](https://en.wikipedia.org/wiki/Domain_name) like [Boot.dev](https://boot.dev). DNS "resolves" those domain names to their associated [IP addresses](https://en.wikipedia.org/wiki/Internet_Protocol) so that web clients can find the server they're looking for.

_Domain names are for humans, IP addresses are for computers._

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/lvjZb5u-1280x513.png)

## How Does DNS Work?

We'll go into more detail on DNS in a future course, but to give you a simplified idea, let's just introduce ICANN. [ICANN](https://www.icann.org/) is a not-for-profit organization that manages DNS for the entire internet.

Whenever your computer attempts to resolve a domain name, it contacts one of ICANN's ["root nameservers"](https://en.wikipedia.org/wiki/Root_name_server) whose address is included in your computer's networking configuration. From there, that nameserver can gather the domain records for a specific domain name from their distributed DNS database.

If you think of DNS as a phonebook, ICANN is the publisher that keeps the phonebook in print and available.