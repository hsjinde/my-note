```python!
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        maxSum = nums[0]
        curSum = 0
        for num in nums:
            curSum += num
            maxSum = max(curSum, maxSum)
            curSum = max(0, curSum)
        return maxSum
```
Time complexity: O(n)
Space complexity: O(1)