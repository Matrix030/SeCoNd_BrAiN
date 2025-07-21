The big difference between relational and non-relational databases is that non-relational databases _nest_ their data. Instead of keeping records on separate tables, they store records _within other records_.

To over-simplify it, you can think of non-relational databases as giant JSON blobs. If a user can have multiple courses, you might just add all the courses to the user record.

```json
{
  "users": [
    {
      "id": 0,
      "name": "Elon",
      "courses": [
        {
          "name": "Biology",
          "id": 0
        },
        {
          "name": "Biology",
          "id": 0
        }
      ]
    }
  ]
}
```

This often results in _duplicate data_ within the database. That's obviously less than ideal, but it does have some benefits that we'll talk about later in the course.

## Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/Ogx4ICa.jpg)

## Non-Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/36gbplf-900x480.jpeg)