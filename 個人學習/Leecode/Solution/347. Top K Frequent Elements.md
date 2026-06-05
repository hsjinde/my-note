
# 347. Top K Frequent Elements

LeetCode 347. Top K Frequent Elements (前 K 個高頻元素)

## 目錄
- 思路總覽 (Overview & Interview Plan)
- 演算法詳解
- 程式碼實作
- 正確性說明
- 複雜度分析
- 常見誤區
- 方法比較與選擇
- 實戰演練
- 面試技巧
- 小結

## 思路總覽 (Overview & Interview Plan)

### 中文解題計畫
- 建立頻率表：遍歷陣列，使用 Hash Map (如 Python `Counter`) 統計每個數字出現次數。
- 策略 A — Min-Heap：維護大小為 $k$ 的最小堆，超過就彈出頻率最小者，留下前 $k$ 高頻元素。
- 策略 B — Bucket Sort：以頻率為索引建桶陣列，從高頻往回掃描收集前 $k$ 個。
- 輸出結果：回傳選出的 $k$ 個元素列表。

### English Interview Plan
- Build a frequency map for all elements.
- Choose approach:
  - Min-Heap: keep a min-heap of size k with (freq, value); pop when size exceeds k.
  - Bucket Sort: array of lists indexed by frequency; scan from highest frequency down.
- Return the collected k elements.

## 演算法詳解

假設輸入 `nums = [1, 1, 1, 2, 2, 3], k = 2`。

1) 統計頻率：掃描得到 `{1: 3, 2: 2, 3: 1}`。
2) 篩選 (以 Min-Heap 為例)：
   - push (3, 1) → heap = [(3, 1)]
   - push (2, 2) → heap = [(2, 2), (3, 1)]
   - push (1, 3) → heap = [(1, 3), (3, 1), (2, 2)]，尺寸超過 k，pop 移除 (1, 3)
   - 最終 heap = [(2, 2), (3, 1)]，答案為 [2, 1] (順序不拘)

## 程式碼實作

### 方法一：Min-Heap (標準解法)

```python
import heapq
from collections import Counter
from typing import List


class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        heap = []

        for num, freq in count.items():
            heapq.heappush(heap, (freq, num))
            if len(heap) > k:
                heapq.heappop(heap)

        return [num for freq, num in heap]
```

### 方法二：Bucket Sort (線性時間優化)

```python
from collections import Counter
from typing import List


class Solution:
    def topKFrequent_bucketSort(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        buckets = [[] for _ in range(len(nums) + 1)]

        for num, freq in count.items():
            buckets[freq].append(num)

        result = []
        for i in range(len(buckets) - 1, 0, -1):
            if buckets[i]:
                for num in buckets[i]:
                    result.append(num)
                    if len(result) == k:
                        return result
        return result
```

## 正確性說明
- 不遺漏：Frequency Map 計數覆蓋所有元素；Bucket Sort 從最大頻率往下檢查，必取高頻。
- 不重複：Hash Map key 唯一，結果來自唯一集合；題目要求回傳元素值而非複製多次。
- 邊界：`k == len(nums)` 返回全部；`len(nums) == 1` 也能正確處理。

## 複雜度分析
- Time
  - Heap：$O(N \log k)$，計數 $O(N)$ + 遍歷唯一元素做 heap 操作。
  - Bucket：$O(N)$，計數 + 建桶 + 從高頻掃描收集。
- Space
  - Heap：Hash Map $O(N)$，heap $O(k)$，總 $O(N)$。
  - Bucket：Hash Map $O(N)$，buckets 陣列 $O(N)$，總 $O(N)$。

## 常見誤區
- Heap 比較順序錯誤：heapq 以 tuple 第一欄排序，應放 `(freq, num)`。
- 誤用 Max-Heap：在 Python 需以負頻率；全部放入再 pop k 次為 $O(N \log N)$，較慢。
- Bucket 長度不足：最高頻率可達 `len(nums)`，需配置 `len(nums) + 1`。

## 方法比較表

| 方法                      | 時間複雜度    | 空間複雜度 | 優點                       | 缺點             |
| ------------------------- | ------------- | ---------- | -------------------------- | ---------------- |
| **Min-Heap**              | $O(N \log k)$ | $O(N)$     | 空間利用率高，k 小時效率好 | 需要維護堆的平衡 |
| **Bucket Sort**           | $O(N)$        | $O(N)$     | 線性時間最優，穩定性好     | 需要額外空間建桶 |
| **Counter.most_common()** | $O(N \log N)$ | $O(N)$     | 代碼最簡潔                 | 時間複雜度較高   |

## 實戰演練

### 測試案例

```python
# Test Case 1: 基本例子
nums = [1, 1, 1, 2, 2, 3]
k = 2
# Expected: [1, 2] (順序無關)

# Test Case 2: 只有一個元素
nums = [1]
k = 1
# Expected: [1]

# Test Case 3: 所有元素頻率相同
nums = [1, 2, 3, 4]
k = 2
# Expected: [任意 2 個元素]

# Test Case 4: k 等於獨特元素數
nums = [1, 1, 1, 2, 2, 3]
k = 3
# Expected: [1, 2, 3]

# Test Case 5: 大數組
nums = list(range(1000)) + [0] * 500 + [1] * 300 + [2] * 200
k = 3
# Expected: [0, 1, 2] (頻率遞減)
```

### 代碼變體：支援多種選擇

```python
class Solution:
    # 變體 1：使用內建 most_common
    def topKFrequent_v1(self, nums: List[int], k: int) -> List[int]:
        return [val for val, _ in Counter(nums).most_common(k)]
    
    # 變體 2：大頂堆（負數技巧）
    def topKFrequent_v2(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        heap = [(-freq, num) for num, freq in count.items()]
        heapq.heapify(heap)
        return [heapq.heappop(heap)[1] for _ in range(k)]
    
    # 變體 3：手動計數（不用 Counter）
    def topKFrequent_v3(self, nums: List[int], k: int) -> List[int]:
        freq_map = {}
        for num in nums:
            freq_map[num] = freq_map.get(num, 0) + 1
        
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in freq_map.items():
            buckets[freq].append(num)
        
        result = []
        for i in range(len(buckets) - 1, -1, -1):
            result.extend(buckets[i])
            if len(result) >= k:
                return result[:k]
        return result
```

## 面試技巧

### 實戰技巧清單
1. **澄清需求**：確認是否需要特定的結果順序；k 的範圍。
2. **空間優化**：若 k 遠小於 N，Min-Heap 更優；反之 Bucket Sort。
3. **邊界檢查**：k == 0、k > len(unique_nums)、nums 為空。
4. **時間複雜度分析**：能否用 $O(N)$ 解？為什麼 $O(N \log k)$ 是下界？
5. **代碼簡潔性**：優先展示 Counter + most_common，再講最優化方案。

### 延伸問題

1. **Variant 1**：若數據是流式輸入，如何設計？
   - 答案：使用 Min-Heap + 動態更新，適合 Top-K 問題的流式版本。

2. **Variant 2**：如何返回出現頻率最低的 K 個元素？
   - 答案：改用 Max-Heap（負數），或反向掃描 Bucket。

3. **Variant 3**：若要求同頻率的元素按值大小排序？
   - 答案：修改 tuple 為 `(freq, -num, num)`，或在結果中排序。

4. **Variant 4**：若 k 為 0 或負數怎麼辦？
   - 答案：添加邊界檢查：`if k <= 0: return []`

5. **Variant 5**：如何支援流式更新？
   - 答案：維護動態計數器，每次新增元素時更新堆。

### 複雜度選擇指南

- **選擇 Bucket Sort**：
  - k 接近 unique 元素數量
  - 需要最快時間複雜度 ($O(N)$)
  - 內存充足

- **選擇 Min-Heap**：
  - k 遠小於 unique 元素數量
  - 優先考慮空間效率
  - 需要動態調整（流式數據）

- **選擇 Counter.most_common()**：
  - 面試時想快速驗證邏輯
  - 代碼簡潔性優先於效率
  - 規模較小的輸入

## 小結
- **核心**：Hash Map 計數 + Top-K 選擇。
- **場景**：資料量極大、流式資料 → Min-Heap；頻率範圍有限且需線性時間 → Bucket Sort。
- **面試重點**：理解權衡、展示多個解法、能從簡到難。
- **延伸**：推薦練習 LeetCode 215. Kth Largest Element in an Array、692. Top K Frequent Words。
- **關鍵洞見**：Top-K 問題通常不需要完全排序，利用 Heap 或 Bucket 可以達到最優複雜度。