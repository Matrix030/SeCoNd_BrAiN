We often need to alter our database schema without deleting it and re-creating it. Imagine if Twitter deleted its database each time it needed to add a feature, that would be a _disaster_! Your account and all your tweets would be wiped out on a daily basis.

Instead, we can use the `ALTER TABLE` statement to make changes in place without deleting any data.

## ALTER TABLE

With SQLite an `ALTER TABLE` statement allows you to:

### 1. Rename a Table or Column

```sql
ALTER TABLE employees
RENAME TO contractors;

ALTER TABLE contractors
RENAME COLUMN salary TO invoice;
```

### 2. Add or DROP a Column

```sql
ALTER TABLE contractors
ADD COLUMN job_title TEXT;

ALTER TABLE contractors
DROP COLUMN is_manager;
```

Unlike some SQL databases, SQLite does not support adding multiple columns in a single `ALTER TABLE` statement. Each column must be added in a separate `ALTER TABLE` command.