SQL is just a query language. You typically use it to interact with a specific database technology. For example:

- [SQLite](https://www.sqlite.org/index.html)
- [PostgreSQL](https://www.postgresql.org/)
- [MySQL](https://www.mysql.com/)
- [CockroachDB](https://www.cockroachlabs.com/)
- [Oracle](https://www.oracle.com/database/)
- etc...

Although many different databases use the SQL _language_, most of them will have their own _dialect_. It's _critical_ to understand that _not_ all databases are created equal. Just because one SQL-compatible database does things a certain way, doesn't mean every SQL-compatible database will follow those exact same patterns.

## We're Using SQLite

In this course, we'll be using [SQLite](https://www.sqlite.org/index.html) specifically. SQLite is great for embedded projects, web browsers, and toy projects. It's lightweight, but has limited functionality compared to the likes of PostgreSQL or MySQL - two of the more common production SQL technologies.

We'll point out to you whenever some functionality we're working with is unique to SQLite!