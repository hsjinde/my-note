# LeetCode 2596. Check Knight Tour Configuration  
(檢查騎士巡遊配置)

## 目錄
- [題目描述 (中文摘要)](#題目描述-中文摘要)
- [思路總覽](#思路總覽)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
  - [原始 DFS 驗證版本](#原始-dfs-驗證版本)
  - [更簡潔版本：位置映射一次掃描](#更簡潔版本位置映射一次掃描)
- [正確性說明](#正確性說明)
- [時間複雜度分析](#時間複雜度分析)
- [空間複雜度分析](#空間複雜度分析)
- [常見誤區與補充](#常見誤區與補充)
- [延伸思考](#延伸思考)
- [小結](#小結)

---

## 題目描述 (中文摘要)
給定一個 `n x n` 的整數矩陣 `grid`，其包含從 `0` 到 `n*n - 1` 的所有整數（恰好一次）。  
`grid[r][c]` 表示騎士在巡遊過程中第 `grid[r][c]` 次到達的格子位置。  
檢查是否存在一條合法的「騎士巡遊」路徑，使得：
- 起點必須是 `grid[0][0] == 0`；
- 對於每一個 `k`（1 ≤ k < n*n），位置 `k` 必須能由位置 `k-1` 透過一次騎士步（象棋中的 L 形走法）到達。

若滿足條件，回傳 `True`，否則回傳 `False`。

---

## 思路總覽
- 核心條件：`grid[0][0]` 必須是 `0`，且每個連續數字 `k-1 -> k` 之間需為合法騎士步。
- 兩種常見作法：
  1. DFS：從 `(0,0)` 開始，依序只走向值為下一步編號的格子；最後應訪問到 `n*n` 個格子。
  2. 位置映射：先把每個數字 `k` 的座標記下，再線性檢查 `k-1` 到 `k` 是否為騎士步，時間更佳、實作更簡潔。

---

## 演算法詳解
1. 騎士步定義  
   從 `(r, c)` 可以到達以下 8 個方向之一：  
   `(±1, ±2)` 與 `(±2, ±1)` 的組合。

2. DFS 作法重點  
   - 先檢查 `grid[0][0] == 0`，否則直接回傳 `False`。  
   - 用 `visited` 標記已符合序號的格子。  
   - 從 `(0,0)`、計數 `count=0` 起，遞迴嘗試 8 個方向中是否存在 `grid[nr][nc] == count+1` 的下一步。  
   - 全部走完後，若 `visited` 大小為 `n*n`，代表存在一條完整巡遊路徑。

3. 位置映射作法重點  
   - 建立 `pos[k] = (r, c)` 的映射。  
   - 檢查 `grid[0][0] == 0`。  
   - 逐一確認 `pos[k-1]` 到 `pos[k]` 是否為騎士步，全部成立即為合法。

---

## 程式碼實作

### 原始 DFS 驗證版本
```python
from typing import List, Set, Tuple

class Solution:
    def checkValidGrid(self, grid: List[List[int]]) -> bool:
        n = len(grid)
        # 起點必須是 0
        if grid[0][0] != 0:
            return False

        directions = [(1, 2), (2, 1), (1, -2), (2, -1),
                      (-1, 2), (-2, 1), (-1, -2), (-2, -1)]
        visited: Set[Tuple[int, int]] = set()

        def dfs(r: int, c: int, count: int) -> None:
            # 若座標越界 / 已訪問 / 或當前格子不等於應有的序號，則停止
            if r < 0 or r >= n or c < 0 or c >= n:
                return
            if (r, c) in visited:
                return
            if grid[r][c] != count:
                return

            visited.add((r, c))
            # 嘗試找下一步（count+1），理論上最多只有一個成立
            for dr, dc in directions:
                dfs(r + dr, c + dc, count + 1)

        dfs(0, 0, 0)
        return len(visited) == n * n
```

### 更簡潔版本：位置映射一次掃描
- 先一次走訪 `grid` 建立數值到座標的映射表 `pos`。
- 線性檢查 `0 -> 1 -> 2 -> ... -> n*n-1` 是否每一步都是合法騎士步。

```python
from typing import List, Tuple

class Solution:
    def checkValidGrid(self, grid: List[List[int]]) -> bool:
        n = len(grid)
        # 起點必須是 0
        if grid[0][0] != 0:
            return False

        # 建立每個編號的位置映射
        pos: List[Tuple[int, int]] = [(-1, -1)] * (n * n)
        for r in range(n):
            for c in range(n):
                pos[grid[r][c]] = (r, c)

        # 檢查連續步驟是否為騎士步
        def is_knight_move(a: Tuple[int, int], b: Tuple[int, int]) -> bool:
            dr, dc = abs(a[0] - b[0]), abs(a[1] - b[1])
            return (dr, dc) in [(1, 2), (2, 1)]

        for k in range(1, n * n):
            if not is_knight_move(pos[k - 1], pos[k]):
                return False
        return True
```

---

## 正確性說明
- DFS 版本：只有符合當前序號 `count` 的格子才會被標記並繼續展開，等同於沿著 `0 -> 1 -> ... -> n*n-1` 的路徑前進；最終若能訪問到 `n*n` 個格子，代表存在合法巡遊路徑。
- 位置映射版本：逐對檢查 `k-1` 到 `k` 是否為騎士步，完整覆蓋了定義要求；所有步驟皆合法則答案為真。

---

## 時間複雜度分析
- DFS 版本：最壞情況下，每個格子最多嘗試 8 個方向，時間複雜度約為 O(n^2) ～ O(8·n^2)。
- 位置映射版本：建立映射 O(n^2)，線性檢查 O(n^2)，總計 O(n^2)。

---

## 空間複雜度分析
- DFS 版本：`visited` 最多 O(n^2)，遞迴深度最壞 O(n^2)（理論上沿著路徑遞增）。
- 位置映射版本：儲存 `pos` 需要 O(n^2) 額外空間，無遞迴棧。

---

## 常見誤區與補充
1. 忘記檢查起點是否為 `0`（`grid[0][0] != 0` 必為 `False`）。
2. 騎士步條件寫錯，應為 `(dr, dc) ∈ {(1,2), (2,1)}` 的排列。
3. DFS 版本沒有及時剪枝或誤用 `visited`，導致錯誤計數或重複拜訪。
4. `n == 1` 的特例：只要 `grid[0][0] == 0` 即為 `True`。

---

## 延伸思考
- 若改為檢查是否存在任意起點的合法巡遊（而非固定 `(0,0)` 為 0），則需枚舉起點或放寬起點條件再驗證序列連續性。
- 可將騎士步集合預先以 `set` 儲存以加速判斷。

---

## 小結
- 本題關鍵是確認從 `0` 到 `n*n-1` 的每一步皆為合法騎士步，且起點為 `(0,0)`。
- 位置映射的線性掃描法更簡潔、效率高；DFS 也可行但實作上需小心訪問與剪枝。