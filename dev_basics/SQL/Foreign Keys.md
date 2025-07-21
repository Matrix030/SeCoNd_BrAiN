Foreign keys are what make relational databases relational! Foreign keys define the relationships _between_ tables. Simply put, a `FOREIGN KEY` is a field in one table that references another table's `PRIMARY KEY`.

## Creating a Foreign Key in SQLite

Creating a `FOREIGN KEY` in SQLite happens at table creation! After we define the table fields and constraints we add a named `CONSTRAINT` where we define the `FOREIGN KEY` column and its `REFERENCES`.

Here's an example:

```sql
CREATE TABLE departments (
    id INTEGER PRIMARY KEY,
    department_name TEXT NOT NULL
);

CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department_id INTEGER,
    CONSTRAINT fk_departments
    FOREIGN KEY (department_id)
    REFERENCES departments(id)
);
```

In this example, an `employee` has a `department_id`. The `department_id` must be the same as the `id` field of a record from the `departments` table. `fk_departments` is the specified name of the [constraint](https://www.sqlite.org/lang_createtable.html#constraint_enforcement).

1. `CONSTRAINT fk_departments`: create a constraint called `fk_departments`
2. `FOREIGN KEY (department_id)`: make this constraint a foreign key assigned to the `department_id` field
3. `REFERENCES departments(id)`: link the foreign field `id` from the `departments` table