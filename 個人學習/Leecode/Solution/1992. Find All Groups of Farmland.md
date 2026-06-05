# LeetCode 1992. Find All Groups of Farmland  
(尋找所有農地群)

## 目錄
- [題目描述 (中文摘要)](#題目描述-中文摘要)
- [思路總覽](#思路總覽)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
  - [DFS 洪水填充版本（以邊界框記錄）](#dfs-洪水填充版本以邊界框記錄)
  - [最佳化版本：掃描矩形邊界（無需 DFS）](#最佳化版本掃描矩形邊界無需-dfs)
- [正確性說明](#正確性說明)
- [時間複雜度分析](#時間複雜度分析)
- [空間複雜度分析](#空間複雜度分析)
- [常見誤區與補充](#常見誤區與補充)
- [延伸思考](#延伸思考)
- [小結](#小結)

---

## 題目描述 (中文摘要)
給定一個二維 0/1 矩陣 `land`，其中 `1` 表示農地、`0` 表示非農地。  
題目保證每塊農地群為一個不相交的「實心矩形」。  
請找出 `land` 中所有農地群，並以每塊矩形的左上角與右下角座標 `[r1, c1, r2, c2]` 回傳。

---

## 思路總覽
- 農地群是彼此不相交且為完整矩形。
- 兩種常見作法：
  1. DFS/BFS 洪水填充：從每個未訪問的 `1` 出發，擴展該連通塊，同時維護當前區塊的最大列、最大行作為右下角坐標。
  2. 掃描矩形邊界：只在遇到農地群的「左上角」時啟動，向右、向下找到右下角，直接記錄一個矩形並（可選）將其區域清零避免重複。

---

## 演算法詳解
1. 遍歷每個格子 `(r, c)`：
   - 若為 `1` 且尚未訪問，代表新農地群起點。
2. DFS/BFS 擴張：
   - 在探索過程中更新當前區塊的右下角 `(max_r, max_c)`。
   - 將所有屬於該農地群的格子標記為已訪問。
3. 完成一個區塊後，加入結果 `[start_r, start_c, max_r, max_c]`。

最佳化掃描法：
- 當 `land[r][c] == 1` 且其上方與左方不是 `1`（或越界）時，`(r, c)` 為某個矩形的左上角。
- 自 `(r, c)` 向右延伸找最右列 `c2`，向下延伸找最下列 `r2`，即右下角 `(r2, c2)`。
- 記錄 `[r, c, r2, c2]`，並（可選）將該矩形內全部置為 `0`，避免後續重複。

---

## 程式碼實作

### DFS 洪水填充版本（以邊界框記錄）
```python
from typing import List, Set, Tuple

class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        row, col = len(land), len(land[0])
        res: List[List[int]] = []
        visited: Set[Tuple[int, int]] = set()
        directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]

        def dfs(r: int, c: int, bottom_right: List[int]) -> None:
            # 越界 / 非農地 / 已訪問 -> 停止
            if r < 0 or r >= row or c < 0 or c >= col:
                return
            if land[r][c] == 0 or (r, c) in visited:
                return

            visited.add((r, c))
            # 更新右下角邊界
            bottom_right[0] = max(bottom_right[0], r)
            bottom_right[1] = max(bottom_right[1], c)

            for dr, dc in directions:
                dfs(r + dr, c + dc, bottom_right)

        for r in range(row):
            for c in range(col):
                if land[r][c] == 1 and (r, c) not in visited:
                    bottom_right = [r, c]  # 動態更新為矩形右下角
                    dfs(r, c, bottom_right)
                    res.append([r, c, bottom_right[0], bottom_right[1]])
        return res
```

### 最佳化版本：掃描矩形邊界（無需 DFS）
- 利用「左上角偵測」只在矩形起點處做延伸，時間與常數更優。

```python
from typing import List

class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        m, n = len(land), len(land[0])
        ans: List[List[int]] = []

        r = 0
        while r < m:
            c = 0
            while c < n:
                if land[r][c] == 1:
                    # 判斷是否為左上角
                    up_is_zero = (r == 0) or (land[r - 1][c] == 0)
                    left_is_zero = (c == 0) or (land[r][c - 1] == 0)
                    if up_is_zero and left_is_zero:
                        # 向右找到最右 c2
                        c2 = c
                        while c2 + 1 < n and land[r][c2 + 1] == 1:
                            c2 += 1
                        # 向下找到最下 r2（需確保整列到 c2 都是 1）
                        r2 = r
                        done = False
                        while r2 + 1 < m and not done:
                            for x in range(c, c2 + 1):
                                if land[r2 + 1][x] == 0:
                                    done = True
                                    break
                            if not done:
                                r2 += 1
                        # 記錄矩形
                        ans.append([r, c, r2, c2])
                        # 將矩形區域清零，避免重複處理
                        for i in range(r, r2 + 1):
                            for j in range(c, c2 + 1):
                                land[i][j] = 0
                        # 跳到 c2 之後
                        c = c2
                c += 1
            r += 1

        return ans
```

---

## 正確性說明
- DFS 版本：探索同一連通塊內的所有 `1`，由於題目保證每個群是矩形，連通塊的最小/最大行列即為矩形邊界。
- 掃描版本：僅在遇到矩形左上角時延伸到右下角，且沿途確認矩形邊界內皆為 `1`，並清零避免重複。

---

## 時間複雜度分析
- DFS 版本：每個格子最多訪問一次，總時間 O(RC)。
- 掃描版本：每個格子至多被掃描與清零一次，總時間 O(RC)。

---

## 空間複雜度分析
- DFS 版本：`visited` 需要 O(RC)；遞迴深度最壞 O(RC)（或 O(min(R, C)) 視形狀）。
- 掃描版本：原地修改，僅使用 O(1) 額外空間。

---

## 常見誤區與補充
1. 越界判定使用 `r<0 or r>=row` 等條件較 `r not in range(row)` 更高效。
2. 若不想改動原矩陣，可用 `visited` 標記；若可改動，清零可省記憶體。
3. 假設不成立（若農地群不是純矩形），上述掃描法需改為通用的 DFS/BFS。

---

## 延伸思考
- 若題目允許矩形相鄰但不相交，掃描法依舊成立（因為左上角條件可唯一定位群起點）。
- 若需要輸出矩形面積或內部座標，可於延伸時同步計算。

---

## 小結
- 本題關鍵：每個農地群都是一塊矩形。
- DFS 洪水填充能正確取得邊界；掃描左上角再擴張的作法更簡潔且常數更低。