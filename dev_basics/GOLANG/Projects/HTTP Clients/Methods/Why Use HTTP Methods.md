The primary purpose of HTTP methods is to indicate to the server what we want to do with the resource we're trying to interact with. At the end of the day, an HTTP method is just a string, like `GET`, `POST`, `PUT`, or `DELETE`, but by _convention_, backend developers write their server code so that the methods correspond with different "CRUD" actions.

The "CRUD" actions are:

- Create
- Read
- Update
- Delete

The bulk of the logic in most web applications is "CRUD" logic. Even a complex social media site is _mostly_ just CRUD. Users create, read, update and delete their accounts. They create, read, update, and delete their posts. _It's CRUD all the way down!_

The 4 most common HTTP methods map nicely to the CRUD actions:

|HTTP Method|CRUD Action|
|---|---|
|GET|Read|
|POST|Create|
|PUT|Update|
|DELETE|Delete|

These mappings follow convention, not law. Some APIs may use `POST` for updating and creating, some might just skip `DELETE`.