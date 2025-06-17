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

