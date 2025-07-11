Once installed, you can use `jq` to parse and manipulate JSON data. Here's a simple example to get you started. Suppose you have a JSON file named `user.json`:

```json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

To extract the `name` field, you would use:

```sh
jq '.name' user.json
# "John"
```

## jq with cURL

`jq` is frequently used with `cURL` to parse JSON responses directly from HTTP requests. For example, to fetch JSON data from an API and extract a field we can use the [`object identifier index`](https://jqlang.github.io/jq/manual/#object-identifier-index):

```sh
curl https://jsonplaceholder.typicode.com/users/1 | jq .username
# "Bret"
```

To get a field from each element in an array you can use the [`array/object value iterator`](https://jqlang.github.io/jq/manual/#array-object-value-iterator) `.[]`, which can in turn be combined with the identifier index:

```sh
curl https://jsonplaceholder.typicode.com/users | jq '.[].username'
# "Bret"
# "Antonette"
# "Samantha"
# "Karianne"
# "Kamren"
# "Leopoldo_Corkery"
# "Elwyn.Skiles"
# "Maxime_Nienow"
# "Delphine"
# "Moriah.Stanton"
```

## Multiple Fields

```sh
curl https://jsonplaceholder.typicode.com/users/1 | jq '.name, .email'
# "Leanne Graham"
# "Sincere@april.biz"
```

`jq` is extremely powerful. We won't cover every feature in this course, but it's a tool you should know about for the future.