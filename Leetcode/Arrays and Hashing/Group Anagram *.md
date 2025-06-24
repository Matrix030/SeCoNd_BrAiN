## 1. Sorting

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        for s in strs:
            sortedS = ''.join(sorted(s))
            res[sortedS].append(s)
        return list(res.values())
```

### Time & Space Complexity

- Time complexity: O(m∗nlog⁡n)O(m∗nlogn)
- Space complexity: O(m∗n)O(m∗n)
Where mm is the number of strings and nn is the length of the longest string.

## 2. Hash Table

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord('a')] += 1
            res[tuple(count)].append(s)
        return list(res.values())
```

### Time & Space Complexity

- Time complexity: O(m∗n)O(m∗n)
- Space complexity:
    - O(m)O(m) extra space.
    - O(m∗n) space for the output list.

Where m is the number of strings and nn is the length of the longest string.

![[Pasted image 20250624153620.png]]
