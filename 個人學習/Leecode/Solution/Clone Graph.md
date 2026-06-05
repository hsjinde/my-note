
```python!
from collections import deque
from typing import Optional

# 定義節點類別
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val  # 節點值
        self.neighbors = neighbors if neighbors is not None else []  # 這個節點的鄰居節點列表

class Solution:
    # 複製圖的函數
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if not node:  # 如果圖為空，則返回空
            return node
        
        visited = {}  # 存儲已訪問過的節點及其克隆節點
        Q = deque([node])  # 使用佇列進行廣度優先搜索
        visited[node] = Node(node.val)  # 將原始節點加入到已訪問字典中，對應的克隆節點值相同
        
        while Q:  # 當佇列不為空時
            n = Q.popleft()  # 從佇列中取出一個節點
            for nei in n.neighbors:  # 遍歷該節點的鄰居節點
                if nei not in visited:  # 如果該鄰居節點尚未被訪問
                    Q.append(nei)  # 將其加入佇列中等待訪問
                    visited[nei] = Node(nei.val)  # 創建該鄰居節點的克隆節點並加入已訪問字典
                visited[n].neighbors.append(visited[nei])  # 將該鄰居節點的克隆節點加入到當前節點的克隆節點的鄰居列表中
        return visited[node]  # 返回複製後的圖的根節點

```
### Time Complexity: O(V + E).
- 深度優先搜索 (DFS) 或廣度優先搜索 (BFS) 遍歷圖的時間複雜度為 O(V + E)，其中 V 是節點的數量，E 是邊的數量。
- 在這個解法中，我們使用了廣度優先搜索來遍歷圖。在最壞的情況下，我們需要訪問每個節點和它的每個鄰居，因此時間複雜度為 O(V + E)。
### Space Complexity: O(V)
- 我們使用了一個字典 visited 來存儲已經訪問過的節點及其對應的克隆節點，以及一個佇列 Q 來進行廣度優先搜索。
- 最壞情況下，我們可能需要存儲所有節點的克隆節點，因此空間複雜度為 O(V)。
- 同時，在進行廣度優先搜索時，可能需要存儲所有的節點到佇列中，因此空間複雜度為 O(V)。
- 綜合起來，總的空間複雜度為 O(V)。