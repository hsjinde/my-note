---
title: Combination Sum II
---

# LeetCode 40. Combination Sum II  
(組合總和 II)

## 目錄
- [題目描述（中文摘要）](#題目描述中文摘要)
- [思路總覽](#思路總覽)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
  - [原始 DFS 回溯版本](#原始-dfs-回溯版本)
  - [優化版本：累加 total、避免 sum 遍歷](#優化版本累加-total避免-sum-遍歷)
- [正確性說明](#正確性說明)
- [時間複雜度分析](#時間複雜度分析)
- [空間複雜度分析](#空間複雜度分析)
- [常見誤區與補充](#常見誤區與補充)
- [延伸思考](#延伸思考)
- [小結](#小結)

---

## 題目描述（中文摘要）
給定一組正整數陣列 `candidates`（元素可重複），和一個目標值 `target`。  
請找出所有**不重複**的組合，使得組合中的數字加總等於 `target`。  
每個數字在每個組合中最多只能使用一次（即每個元素只能選或不選，不可重複選取）。

例如：  
輸入: `candidates = [10,1,2,7,6,1,5]`, `target = 8`  
可形成：  
`[1,1,6], [1,2,5], [1,7], [2,6]` 共 4 個組合。

---

## 思路總覽
- 先將 `candidates` 排序，便於去除重複組合。
- 使用「回溯（Backtracking）」遞迴遍歷所有可能選取方式：
    1. 每層遞迴沿著「不選/選」展開分支。
    2. 若遇到連續重複元素，跳過同層重複，避免重複組合。
    3. 組合和等於 `target` 時記錄結果。
    4. 組合和超過 `target` 則停止分支（剪枝）。
## English ver
1. Sort the candidate numbers.
2. Backtracking recursion:
    - At each step, iterate through candidates starting from the current index.
    - Skip duplicates: If candidates[i] == candidates[i-1] and i > start, continue to the next.
    - Choose the number: Add candidates[i] to the current combination, recurse with i+1 and updated sum.
    - Backtrack: Remove the last added number after recursion.
3. Record result if the current sum equals the target.
4. Prune branch if the current sum exceeds the target.

---

## 演算法詳解

1. **排序**  
   先對 `candidates` 排序，確保重複元素相連。

2. **回溯遞迴**  
    - 設定一個遞迴函數 `backtrack(comb, start)`：
        - `comb`：目前組合。
        - `start`：目前遞迴起始索引，確保每個元素最多使用一次。
    - 每層循環從 `start` 到末尾，嘗試加入每個元素。
    - 若遇到 `i > start and candidates[i] == candidates[i-1]`，跳過，避免同層重複。
    - 若目前組合和等於 `target`，記錄答案。
    - 若組合和超過 `target`，結束分支。
    - 遞迴前 `comb.append(...)`，遞迴後 `comb.pop()` 回溯。

---

## 程式碼實作

### 原始 DFS 回溯版本
```python
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        candidates.sort()
        def backtrack(comb, start):
            if sum(comb) == target:
                res.append(comb.copy())
                return
            elif sum(comb) > target:
                return
            for i in range(start, len(candidates)):
                if i > start and candidates[i] == candidates[i - 1]:
                    continue
                comb.append(candidates[i])
                backtrack(comb, i + 1)
                comb.pop()
        backtrack([], 0)
        return res
```

### Combination Sum II 決策樹說明

#### 關鍵說明

在 Combination Sum II 題目中，為避免組合重複，回溯時需加上：

```python
if i > start and candidates[i] == candidates[i - 1]:
    continue
```

這段判斷會**跳過同層的重複數字**，確保每個組合只被計算一次。

---

#### 決策樹簡單示例

假設 `candidates = [1, 1, 2]`，`target = 3`  
排序後：`[1, 1, 2]`

決策樹展開如下：

```
start=0
[]
 ├─ i=0 → [1]              # 選第一個 1
 │     ├─ i=1 → [1,1]      # 可以用第二個 1（不同層）
 │     │     ├─ i=2 → [1,1,2] (超過 target)
 │     │
 │     └─ i=2 → [1,2]      # 有效解
 │
 ├─ i=1 → (跳過)           # 🚫 因為 i > start 且 candidates[1] == candidates[0]
 │
 └─ i=2 → [2]              # 單獨選 2
```

### 優化版本：累加 total、避免 sum 遍歷
- 用 total 變數儲存目前總和，效率更高。
```python
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        candidates.sort()
        def backtrack(comb, start, total):
            if total == target:
                res.append(comb.copy())
                return
            elif total > target:
                return
            for i in range(start, len(candidates)):
                if i > start and candidates[i] == candidates[i - 1]:
                    continue
                comb.append(candidates[i])
                backtrack(comb, i + 1, total + candidates[i])
                comb.pop()
        backtrack([], 0, 0)
        return res
```

---

## 正確性說明
- 排序後跳過同層重複，保證每組解唯一、不重複。
- 遞迴時只向後遞增索引，確保每元素最多用一次。
- 剪枝（和超過 target）提升效率。

---

## 時間複雜度分析
- 最壞情況下，每個元素都可選或不選，組合數為 $O(2^n)$，n 為陣列長度。
- 若含多個重複元素，實際組合數會下降。

## 空間複雜度分析
- 遞迴深度最多 $O(n)$。
- 組合暫存空間 $O(n)$。

---

## 常見誤區與補充
1. **遺漏排序**：未排序將無法正確去除重複組合。
2. **未跳過同層重複**：會產生重複解。
3. **sum(comb) 效率差**：可用 total 參數優化。

---

## 延伸思考
- 若元素可重複選取（即 Combination Sum I），遞迴時可從同一位置繼續選。
- 若要求所有組合結果排序，可在答案記錄後排序 res。

---

## 小結
- 本題關鍵：排序 + 跳過同層重複元素。
- 回溯法枚舉所有可能組合，遇到重複或超 target 剪枝。
- 時間複雜度 $O(2^n)$；空間 $O(n)$。

---