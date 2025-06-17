- `bool`: a boolean value, either `true` or `false`
- `string`: a sequence of characters
- `int`: a signed integer
- `float64`: a floating-point number
- `byte`: exactly what it sounds like: 8 bits of data

## Declaring a Variable the Sad Way

```go
var mySkillIssues int
mySkillIssues = 42
```
The first line, `var mySkillIssues int`, defaults the `mySkillIssues` variable to its zero value, `0`. On the next line, `42` overwrites the zero value.