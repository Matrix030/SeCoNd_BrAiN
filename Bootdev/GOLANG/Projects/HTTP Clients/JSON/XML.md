We can't talk about JSON without mentioning [XML](https://en.wikipedia.org/wiki/XML#:~:text=Extensible%20Markup%20Language%20\(XML\)%20is,%2Dreadable%20and%20machine%2Dreadable.). `XML`, or "Extensible Markup Language" is a text-based format for representing structured information, like JSON - but different (and a bit more verbose).

## XML Syntax

XML is a markup language like [HTML](https://en.wikipedia.org/wiki/HTML), but it's more generalized in that it does _not_ use predefined tags. Just like how in a JSON object keys can be called anything, XML tags can also have any name.

XML representing a movie:

```xml
<root>
  <id>1</id>
  <genre>Action</genre>
  <title>Iron Man</title>
  <director>Jon Favreau</director>
</root>
```

The same data in JSON:

```json
{
  "id": "1",
  "genre": "Action",
  "title": "Iron Man",
  "director": "Jon Favreau"
}
```
