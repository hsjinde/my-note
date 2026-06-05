# LeetCode 1905. Count Sub Islands  
(子島嶼計數)

## 目錄
- [題目描述 (中文摘要)](#題目描述-中文摘要)
- [思路總覽](#思路總覽)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
  - [DFS 回溯版本](#dfs-回溯版本)
- [正確性說明](#正確性說明)
- [時間複雜度分析](#時間複雜度分析)
- [空間複雜度分析](#空間複雜度分析)
- [常見誤區與補充](#常見誤區與補充)
- [延伸思考](#延伸思考)
- [小結](#小結)

---

## 題目描述 (中文摘要)
給定兩個同樣大小的二維網格 `grid1` 和 `grid2`，  
其中值為 `1` 代表陸地，`0` 代表水域。  
如果在 `grid2` 中的一塊陸地區域，其所有格子都對應到 `grid1` 中也是陸地，  
則這塊區域稱為「子島嶼」。請返回 `grid2` 中子島嶼的數量。

範例：  
```
grid1 = [
 [1,1,1,0,0],
 [0,1,1,1,1],
 [0,0,0,0,0],
 [1,0,0,0,0],
 [1,1,0,1,1]
]
grid2 = [
 [1,1,1,0,0],
 [0,0,1,1,1],
 [0,1,0,0,0],
 [1,0,1,1,0],
 [0,1,0,1,1]
]
輸出：3
```

---

## 思路總覽
- 我們使用 DFS 遍歷 `grid2` 中的每一塊陸地區域。
- 遍歷過程中同時檢查對應的 `grid1` 索引：
  - 若 `grid2` 是陸地，但 `grid1` 對應位置是水域，則整個區域不算子島嶼。
- 將已訪問過的格子標記，避免重複計數。
- 每遍歷完一個 DFS 區域且符合子島嶼條件時，計數器加一。

---

## 演算法詳解
1. **維度與方向**  
   取得網格行數 `row` 和列數 `col`，並預先定義上下左右四個方向向量 `direction`。

2. **已訪問標記**  
   使用 `visited` 集合存儲已走訪過的 `(r, c)`，防止重複遍歷。

3. **深度優先搜尋 (DFS)**  
   定義函式 `dfs(r, c)`：
   - 邊界或水域檢查：若 `(r, c)` 超出範圍、`grid2[r][c] == 0` 或已在 `visited` 中，直接回傳 `True`（不影響父層判斷）。
   - 標記訪問：`visited.add((r, c))`。
   - 子島旗標：若 `grid1[r][c] == 0`，則此區域不是子島嶼，將 `res = False`。
   - 遍歷四個方向，採用短路與邏輯與合併當前結果：
     ```
     res = dfs(nr, nc) and res
     ```
   - 回傳此區域所有遞迴結果的邏輯與值 `res`。

4. **主迴圈計數**  
   對每個格子 `(r, c)`：
   - 若 `grid2[r][c] == 1` 且尚未訪問，
   - 呼叫 `dfs(r, c)`，若回傳 `True` 則計數 `count += 1`。

---

## 程式碼實作

### DFS 回溯版本
```python
from typing import List

class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        row, col = len(grid1), len(grid1[0])
        direction = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        visited = set()

        def dfs(r: int, c: int) -> bool:
            # 邊界或水域或已訪問則不影響結果
            if (r < 0 or r >= row or
                c < 0 or c >= col or
                grid2[r][c] == 0 or
                (r, c) in visited):
                return True

            visited.add((r, c))
            # 假設是子島嶼，若發現對應 grid1 是水域則標記 False
            res = (grid1[r][c] == 1)

            # 遞迴檢查四個方向
            for dr, dc in direction:
                nr, nc = r + dr, c + dc
                # 只有當所有方向都回傳 True，res 才是 True
                res = dfs(nr, nc) and res

            return res

        count = 0
        # 遍歷 grid2 中的每個陸地起點
        for r in range(row):
            for c in range(col):
                if grid2[r][c] == 1 and (r, c) not in visited:
                    if dfs(r, c):
                        count += 1
        return count
```

---

## 正確性說明
- DFS 每塊區域僅走訪一次，`visited` 防止重複。
- 只要該區域在任何一格對應 `grid1` 是水域，即整個區域不計入子島嶼。
- 全程使用邏輯與 (`and`) 保證所有遞迴支線皆為真才能視作子島嶼。

---

## 時間複雜度分析
- 每個格子最多被 DFS 訪問一次，整體時間複雜度 O(`row * col`)。

---

## 空間複雜度分析
- `visited` 集合與遞迴棧最壞皆為 O(`row * col`)。

---

## 常見誤區與補充
1. **邊界條件忘寫**：未檢查 `(r, c)` 超出範圍導致錯誤。
2. **漏標記訪問**：未即時將格子加入 `visited`，易產生無限遞迴。
3. **邏輯與順序**：先做 DFS，再與當前狀態取 `and`，否則短路可能漏訪。

---

## 延伸思考
- 改用 BFS 也可達成相同效果，將 `visited` 和子島嶼判斷集成於佇列處理過程中。
- 若 `grid1` 也需統計其他信息，可在 DFS 中累計額外統計值。

---

## 小結
- 本題核心：對 `grid2` 中每塊陸地區域做 DFS，並檢查覆蓋於 `grid1` 是否全為陸地。
- 善用 `visited` 防重訪及邏輯與確保整片區域符合子島嶼條件。