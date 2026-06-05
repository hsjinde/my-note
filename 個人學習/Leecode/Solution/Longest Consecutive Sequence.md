遍歷nums中的每一個值，檢查該值-1是否存在，如果存在表示長度+1。

```python=
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0
        for n in nums:
            if n-1 not in numSet:
                long = 0
                while (n+long) in numSet:
                    long +=1
                longest = max(long, longest)
        return longest
```
#### Time complexity: $O(n)$


#### Space complexity: $O(n)$
beacase of numSet