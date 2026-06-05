
```python=
class Solution:
    def findMin(self, nums: List[int]) -> int:
        ans = nums[0]
        l, r = 0, len(nums) - 1
        if nums[l] < nums[r]:
            return nums[l]
        while(l <= r):
            if nums[l] < nums[r]:
                ans = min(ans, nums[l])
                break
            m = (l + r) // 2 
            ans = min(ans, nums[m])
            if nums[m] >= nums[l]:
                l = m + 1
            else:
                r = m -1
        return ans
```


![SSHObxM](https://hackmd.io/_uploads/BJk-qo_MR.png)

則 nums[M] 根據 nums[M] 與 nums[L] 大小關係會有兩種情況

1. nums[M] ≥ nums[L] , 要逼近 minimum 需要更新 L = M + 1
![kidWm9b](https://hackmd.io/_uploads/SJOAjiOGA.png)

2. nums[M] < nums[L], 要逼近 minimum 需要更新 R = M - 1
![SAzkFSX](https://hackmd.io/_uploads/HJTJns_G0.png)


#### Time complexity: $O(logn)$
#### Space complexity:$(1)$





# Find Minimum in Rotated Sorted Array - 二分法邊界差異

這份文件對比 `r = m` 與 `r = m - 1` 兩種寫法的差異，並列出在不同旋轉情況下的結果。

---

```python=
class Solution:
    def findMin(self, nums: List[int]) -> int:
        l, r = 0, len(nums) - 1
        while l < r:
            if nums[l] < nums[r]:
                return nums[l]
            m = (l + r) // 2
            if nums[m] >= nums[l]:
                l = m + 1
            else:
                r = m 
        return nums[l]              

        
```


## 演算法邏輯
- **左半有序**：`nums[m] >= nums[l] → l = m + 1`
- **右半有序**：`nums[m] < nums[l]`
  - 正確：`r = m`
  - 錯誤：`r = m - 1`（可能跳過最小值）

---

## 範例對比

### 測資一：`nums = [4,5,6,7,0,1,2]`
- `r = m` → ✅ 找到 `0`
- `r = m - 1` → ✅ 找到 `0`

### 測資二：`nums = [5,1,2,3,4]`
- `r = m` → ✅ 找到 `1`
- `r = m - 1` → ✅ 找到 `1`

### 測資三：`nums = [2,3,4,5,1]`
- `r = m` → ✅ 找到 `1`
- `r = m - 1` → ✅ 找到 `1`

### 測資四：`nums = [3,1,2]`
- `r = m` → ✅ 找到 `1`
- `r = m - 1` → ❌ 錯誤 → 輸出 `3`

---

## 結論
- 使用 **`r = m`** 才能保證不會錯過最小值。  
- 使用 **`r = m - 1`** 在某些情況下（例如 `nums = [3,1,2]`）會錯誤，因為直接把 `m` 這個可能的最小值丟掉了。  

---
