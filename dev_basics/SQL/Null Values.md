In SQL, a cell with a `NULL` value indicates that the value is _missing_. A `NULL` value is _very_ different from a _zero_ value.

## Constraints

When creating a table we can define whether or not a field _can_ or _cannot_ be `NULL`, and that's a kind of `constraint`.

We will cover constraints in more detail soon, for now, let's focus on `NULL` values.

## Tip

Use the `*` (wildcard) syntax to select _all_ fields.

## Observe

Notice that both the `transaction_type` and `was_successful` fields have `NULL` values in all 3 records in the table (nulls are represented by blank cells in our system). That's because we ran our migration in the previous exercise _after_ the 3 records were created!