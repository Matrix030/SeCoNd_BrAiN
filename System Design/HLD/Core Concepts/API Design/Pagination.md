---
tags: [system-design, hld, api]
aliases: ["API Pagination"]
---

# Pagination

1) When you're dealing with large datasets, you can't return everything at once. 
2) Imagine an API that returns all events ever created, that could be millions of records which would be many gigabytes of data.
3) Instead, you need pagination to break large result sets into manageable chunks. There are two main approaches to pagination: **offset-based** and **cursor-based**.

## Offset-based Pagination

1) Offset-based pagination is the simplest approach and used by most websites.
2) You specify how many records to skip and how many to return: /events?offset=20&limit=10 gets records 21-30.
3) This is intuitive and easy to implement, but it has problems with large datasets. 
4) If someone adds a new event while you're paginating through results, you might see duplicates or miss records as the data shifts.

## Cursor-based Pagination

Cursor-based pagination solves this by using a pointer to a specific record instead of counting from the beginning. Here's how it works in practice:

First request: /events?limit=10 Response includes the events plus a cursor pointing to the last record:

```
{
  "events": [...],
  "next_cursor": "cmd9atj3p000007ky19w1dpy2"
}
```

Next request: /events?cursor=cmd9atj3p000007ky19w1dpy2&limit=10

1) The cursor is typically an encoded reference to a specific record (like an ID or timestamp).
2) This is more stable because it's not affected by new records being added, but it's harder to implement features like "jump to page 5."
3) In the example, `cmd9atj3p000007ky19w1dpy2` is the id of the last event in the first page.
4) offset-based pagination is usually fine unless you're dealing with real-time data or the there are about high-volume scenarios. Most interviewers care more about whether you remembered to include pagination than which specific approach you choose.

## Related

- [[> API Design]] — back to the API Design section MOC
- [[REST API Design]] — pagination is most commonly applied to REST list endpoints
- [[API Versioning]] — another common API pattern
