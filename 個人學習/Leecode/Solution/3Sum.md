# 方法一 Brute Force
```python!
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        ans=set()
        nums.sort()
        n=len(nums)
        for i in range(n-2):
            for j in range(i+1,n-1):
                for k in range(j+1,n):
                    temp=nums[i]+nums[j]+nums[k]
                    if temp==0:
                        ans.add((nums[i],nums[j],nums[k]))
        return ans
```

# 方法二 利用兩個pointer (最佳解)
- 因為要找出unique triple，因此要先排序，相同的的數字才會再一起
- 如果總和 < 0 -> 左pointer + 1
- 如果總和 > 0 -> 左pointer - 1
```python!
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        res = set()
        for i in range(n-2):
            l = i + 1
            r = n - 1
            while (l < r):
                total = nums[i] + nums[l] +nums[r]
                if total < 0:
                    l += 1
                elif total > 0:
                    r -= 1
                else:
                    res.add((nums[i] , nums[l] ,nums[r]))
                    l += 1
        return res
```
Time Complexcity: O(n^2)