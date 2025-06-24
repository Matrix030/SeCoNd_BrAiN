## 1. Brute Force

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] == nums[j]:
                    return True
        return False
```

### Time & Space Complexity

- Time complexity: O(n2)O(n2)
- Space complexity: O(1)O(1)

## 2. Sorting

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return True
        return False
```

### Time & Space Complexity

- Time complexity: O(nlog⁡n)O(nlogn)
- Space complexity: O(1)O(1) or O(n)O(n) depending on the sorting algorithm.
## 3. Hash Set

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)
## 4. Hash Set Length

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        return len(set(nums)) < len(nums)
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)


FINAL ANSWER
```python
class Solution:
	def containsDuplicate(self, nums: List[int]) -> bool:
		check_dict = {}
		for num in nums:
		# check if num in is in check_dict:
		# if yes += 1 to that key
		# if no create a new key using .get('key', default_value)
		# if any of the key has a value > 1 then return True.
		# if the number of keys is the same as the length of the list then return False.
			check_dict.setdefault(num, 0)
		if len(check_dict) == len(nums):
			return False
		else:
			return True
```

