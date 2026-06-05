```python!
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # 如果網格是空的，直接返回0
        if not grid:
            return 0
        
        # 初始化島嶼計數器和訪問過的節點集合
        island_count = 0
        visited = set()
        rows, cols = len(grid), len(grid[0])
        
        # 深度優先搜索函數，標記所有連通的'1'
        def dfs(r, c):
            # 如果超出邊界或者遇到'0'或者該節點已訪問過，直接返回
            if (
                r not in range(rows)
                or c not in range(cols)
                or grid[r][c] == '0'
                or (r, c) in visited
            ):
                return 
            # 標記當前節點為已訪問
            visited.add((r, c))
            # 定義四個方向（上下左右）
            directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
            # 遍歷每個方向，遞歸地訪問相鄰的節點
            for dr, dc in directions:
                dfs(r + dr, c + dc)
        
        # 遍歷網格中的每一個節點
        for r in range(rows):
            for c in range(cols):
                # 如果遇到一個未訪問過的'1'，啟動DFS並計算新的島嶼
                if grid[r][c] == '1' and (r, c) not in visited:
                    dfs(r, c)
                    # 每次DFS結束後，島嶼計數器加1
                    island_count += 1
        
        # 返回總的島嶼數量
        return island_count

```
# Complexity
**Time complexity: (O(M * N))**
where (M) is the number of rows and (N) is the number of columns in the grid. We visit each cell once.
**Space complexity: (O(M * N))** 
for the visit set, where (M) is the number of rows and (N) is the number of columns in the grid. Additionally, the recursive stack space for DFS can be at most (O(M \times N)) in the worst case when the grid forms a single island. Therefore, the overall space complexity is (O(M * N)).


[參考_yt](https://www.youtube.com/watch?v=pV2kpPD66nE&ab_channel=NeetCode)


