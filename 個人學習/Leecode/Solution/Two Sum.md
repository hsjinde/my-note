# Brute Force
```python!
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```
Time complexity: O($n^2$)
space complexity: O(1)

# 計算差

```python!
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 遍历列表中的每个数字
        for i in range(len(nums)):
            # 计算当前数字的补数
            dif = target - nums[i]
            # 检查补数是否在列表中
            if dif in nums:
                # 如果补数存在，则找到其索引
                j = nums.index(dif)
                # 确保不是同一个元素的索引
                if i != j:
                    # 返回找到的两个索引
                    return [i, j]

```
Time complexity: O(n)
space complexity: O(1)

# Hash Table
```python!
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}  # 创建一个空字典用于存储数字及其索引
        for i in range(len(nums)):  # 遍历列表中的每个数字
            dif = target - nums[i]  # 计算当前数字的补数
            if dif not in dic:      # 如果补数不在字典中
                dic[nums[i]] = i    # 将当前数字及其索引添加到字典中
            else:
                return [i, dic[dif]]  # 如果补数在字典中，返回当前索引和补数的索引

```
Time complexity: O(n)
space complexity: O(n)