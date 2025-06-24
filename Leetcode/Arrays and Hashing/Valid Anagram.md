## 1. Sorting

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }
    
    sRunes, tRunes := []rune(s), []rune(t)
    sort.Slice(sRunes, func(i, j int) bool { 
        return sRunes[i] < sRunes[j] 
    })
    sort.Slice(tRunes, func(i, j int) bool { 
        return tRunes[i] < tRunes[j] 
    })
    
    for i := range sRunes {
        if sRunes[i] != tRunes[i] {
            return false
        }
    }
    return true
}
```

### Time & Space Complexity

- Time complexity: O(nlog⁡n+mlog⁡m)O(nlogn+mlogm)
- Space complexity: O(1)O(1) or O(n+m)O(n+m) depending on the sorting algorithm.

> Where nn is the length of string ss and mm is the length of string tt.

## 2. Hash Map

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    countS, countT := make(map[rune]int), make(map[rune]int)
    for i, ch := range s {
        countS[ch]++
        countT[rune(t[i])]++
    }

    for k, v := range countS {
        if countT[k] != v {
            return false
        }
    }
    return true
}
```

### Time & Space Complexity

- Time complexity: O(n+m)O(n+m)
- Space complexity: O(1)O(1) since we have at most 2626 different characters.

> Where nn is the length of string ss and mm is the length of string tt.

## 3. Hash Table (Using Array)

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    count := [26]int{}
    for i := 0; i < len(s); i++ {
        count[s[i]-'a']++
        count[t[i]-'a']--
    }

    for _, val := range count {
        if val != 0 {
            return false
        }
    }
    return true
}
```

### Time & Space Complexity

- Time complexity: O(n+m)
- Space complexity: O(1) since we have at most 26 different characters.

## FINAL
```python
class Solution:

def isAnagram(self, s: str, t: str) -> bool:
#check if the lens are same, if there not then its impossible to be a palindrome
if len(s) != len(t):
return False

count_s, count_t = {}, {} #create hashmap
#for O(1) memory
for i in range(len(t)):
	#.get(value, default_value)
	count_s[s[i]] = 1 + count_s.get(s[i], 0)
	count_t[t[i]] = 1 + count_t.get(t[i], 0)

return count_s == count_t
```