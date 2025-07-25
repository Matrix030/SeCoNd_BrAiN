There are times we need to group data based on specific values.

SQL offers the `GROUP BY` clause which can group rows that have similar values into "summary" rows. It returns one row for each group. The interesting part is that each group can have an aggregate function applied to it that operates only on the grouped data.

## Example of GROUP BY

Imagine that we have a database with songs and albums, and we want to see how many songs are on each album. We can use a query like this:

```sql
SELECT album_id, count(song_id)
FROM songs
GROUP BY album_id;
```

This query retrieves a count of all the songs on each album. One record is returned per album, and they each have their own `count`.