Many languages allow multiple values to be returned from a function. For example, in Python this works:

```python
def get_user():
    return "name@domain.com", 21, "active"

email, age, status = get_user()
```

However, in JavaScript, _that's not allowed_!

```js
function getUser() {
  return "name@domain.com", 21, "active";
  // DON'T DO THIS
  // it only returns 'active'
}
```

Strangely, the JavaScript code above won't actually _throw_ any sort of error, it will just silently return the `"active"` string.... which is unintuitive behavior that you probably didn't want. **You can only return one value from a function in JavaScript**!

To get around this, most developers return an object that _contains_ the values they want to return. **We'll cover objects later**.