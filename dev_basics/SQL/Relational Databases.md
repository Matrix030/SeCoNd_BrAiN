We have been using the term _relational_ quite a bit, it's time we actually go over what that means! A _relational_ database is a type of database that stores data so that it can be easily related to other data. For example, a `user` can have many `tweets`. There's a relationship between a `user` and their `tweet`.

In a relational database:

1. Data is typically represented in "tables".
2. Each table has "columns" or "fields" that hold attributes related to the record.
3. Each row or entry in the table is called a [record](https://en.wikipedia.org/wiki/Record_\(computer_science\)).
4. Typically, each record has a unique `Id` called the [primary key](https://en.wikipedia.org/wiki/Primary_key).

## Example Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/Ogx4ICa-1015x593.jpg)

Here is an example of a small relational database. This database has 3 tables, `Students`, `Courses`, and `StudentCourses`. The `StudentCourses` table manages the relationship between the `Students` and the `Courses` tables.

## Example 1: Sam

- Sam has an `Id` of `1`
- We can find Sam's courses by looking in the `StudentCourses` for the records that match his `StudentId`

## Example 2: ASP.NET MVC

- `ASP.NET MVC` has an `Id` of 2
- We can find all the students enrolled in the ASP.NET MVC course by checking the `StudentCourses` table