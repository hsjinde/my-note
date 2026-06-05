# 方法一 Burte Forece
暴力法的思路是對陣列中的每個元素進行迴圈遍歷，並對陣列中的每個其他元素進行比較。如果找到任何重複，則返回true，否則返回false。
```python=
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        n = len(nums)
        for i in range(n - 1):
            for j in range(i + 1, n):
                if nums[i] == nums[j]:
                    return True
        return False
```

該方法的時間複雜度為O(n^2)，其中n是陣列的長度。因此，該方法對於大型陣列不高效 -> TLE(Time Limit Exceeded)

# 方法二 Sorting
另一種方法是將陣列排序，然後檢查相鄰的元素是否相同。如果找到任何重複，則返回true，否則返回false。

```python= 
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums.sort()
        n = len(nums)
        for i in range(1, n):
            if nums[i] == nums[i - 1]:
                return True
        return False
```
該方法的時間複雜度為O(n log n)，其中n是陣列的長度。

# 方法三 Hash Set
```python=
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

該方法的時間複雜度為O(n)，其中n是陣列的長度。

# 方法四 Hash Map
```python=
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = {}
        for num in nums:
            if num in seen and seen[num] >= 1:
                return True
            seen[num] = seen.get(num, 0) + 1
        return False
```
該方法的時間複雜度為O(n)，其中n是陣列的長度。

[本篇參考](https://leetcode.com/problems/contains-duplicate/solutions/3672475/4-method-s-c-java-python-beginner-friendly/)

# 我的方法
```python!
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        #return len(set(nums)) != len(nums)
```
該方法的時間複雜度為O(n)，其中n是陣列的長度。
[資料結構對於時間複雜度的影響](/7X-Zc_I0QG-HdVOj8rVkmA?view)