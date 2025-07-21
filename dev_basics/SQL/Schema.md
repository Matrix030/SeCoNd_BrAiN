We've used the word _schema_ a few times now, let's talk about what that word means. A database's [schema](https://www.ibm.com/think/topics/database-schema) describes how data is organized within it.

Data types, table names, field names, constraints, and the relationships between all of those entities are part of a database's _schema_.

## There Is No Perfect Way to Architect a Database Schema

When designing a database schema there typically isn't a "correct" solution. We do our best to choose a sane set of tables, fields, constraints, etc that will accomplish our project's goals. Like many things in programming, different schema designs come with different tradeoffs.

## How Do We Decide on a Sane Schema Architecture?

Let's use _CashPal_ as an example. One very important decision that needs to be made is to decide which table will store a user's balance! As you can imagine, ensuring our data is accurate when dealing with money is _super_ important. We want to be able to:

- Keep track of a user's current balance
- See the historical balance at any point in the past
- See a log of which transactions changed the balance over time

There are many ways to approach this problem. For our first attempt, let's try the _simplest schema that fulfills our project's needs_.