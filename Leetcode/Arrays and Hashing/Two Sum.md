## 1. Brute Force

```go
func twoSum(nums []int, target int) []int {
    for i := 0; i < len(nums); i++ {
        for j := i + 1; j < len(nums); j++ {
            if nums[i] + nums[j] == target {
                return []int{i, j}
            }
        }
    }
    return []int{}
}
```

### Time & Space Complexity

- Time complexity: O(n2)O(n2)
- Space complexity: O(1)O(1)
## 2. Sorting

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        A = []
        for i, num in enumerate(nums):
            A.append([num, i])
        
        A.sort()
        i, j = 0, len(nums) - 1
        while i < j:
            cur = A[i][0] + A[j][0]
            if cur == target:
                return [min(A[i][1], A[j][1]), 
                        max(A[i][1], A[j][1])]
            elif cur < target:
                i += 1
            else:
                j -= 1
        return []
```

### Time & Space Complexity

- Time complexity: O(nlog⁡n)O(nlogn)
- Space complexity: O(n)O(n)
## 3. Hash Map (Two Pass)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        indices = {}  # val -> index

        for i, n in enumerate(nums):
            indices[n] = i

        for i, n in enumerate(nums):
            diff = target - n
            if diff in indices and indices[diff] != i:
                return [i, indices[diff]]
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)
## 4. Hash Map (One Pass)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index

        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)
![[Pasted image 20250624153130.png]]