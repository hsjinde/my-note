# LeetCode 226. Invert Binary Tree (翻轉二元樹)

## 目錄
- [思路總覽 (Overview & Interview Plan)](#思路總覽-overview--interview-plan)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
- [正確性說明](#正確性說明)
- [複雜度分析](#複雜度分析)
- [常見誤區](#常見誤區)
- [小結](#小結)

---

## 思路總覽 (Overview & Interview Plan)

### 中文解題計畫
這是一道非常經典的二元樹 (Binary Tree) 操作考題。核心目標是將整棵樹產生鏡像反轉。面試時的最佳策略是從「遞迴 (Recursion)」著手：
1. **確立終止條件 (Base Case)**：當前節點為空時，直接返回。
2. **核心遞迴邏輯 (Recursive Step)**：對於任何一個非空節點，我們需要交換它的左子樹 (Left Subtree) 與右子樹 (Right Subtree)。
3. **狀態更新**：利用 Python 優雅的 Tuple Unpacking 特性，同時對左右子節點發起遞迴呼叫，並將回傳的反轉結果直接賦值給相反的指標，完成交換。

### English Interview Plan
* **Approach**: We will use Depth-First Search (DFS) to traverse the binary tree.
* **Core Logic**: The problem asks for the mirror image of a binary tree. To achieve this, we need to swap the left and right children of every single node in the tree.
* **Execution**: We apply a post-order or pre-order traversal recursively. At each node, we recursively call the `invertTree` function on its left and right children. Once the inverted subtrees are returned, we reassign them to the opposite child pointers (swap them).
* **Base Case**: If the current node is `null`, we simply return `null` to halt the recursion.

---

## 演算法詳解

我們可以把這棵樹想成一個由上往下的結構，遞迴的過程本質上就是走訪每個節點並執行「左右互換」的動作。

假設有一棵樹：
      4
    /   \
   2     7
  / \   / \
 1   3 6   9

執行步驟拆解：
1. 來到根節點 `4`，準備交換它的左右子樹（節點 `2` 與節點 `7`）。
2. 但在交換之前，必須先將子樹內部也翻轉。因此遞迴進入左子樹 `2` 與右子樹 `7`。
3. 對於節點 `2`，將其左右子節點 `1` 和 `3` 交換，變成 `3` 和 `1`。
4. 對於節點 `7`，將其左右子節點 `6` 和 `9` 交換，變成 `9` 和 `6`。
5. 最後回到根節點 `4`，將翻轉後的整個左子樹與右子樹互換。

翻轉後：
      4
    /   \
   7     2
  / \   / \
 9   6 3   1

你所提供的寫法非常精練，透過 `root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)` 一行代碼，同時完成了「遞迴深入」與「指標交換」兩個動作。

---

## 程式碼實作

[原始 DFS 遞迴版本]

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 處理終止條件 (Base Case)
        if not root:
            return 
        
        # 核心邏輯：左右子樹同時進行遞迴翻轉，並交換指標
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        
        return root


[延伸優化：BFS 迭代版本 (Iterative BFS)]
在面試中，面試官經常會追問：「如果樹的深度非常深，導致遞迴堆疊溢位 (Stack Overflow) 怎麼辦？」這時可以提出使用佇列 (Queue) 的層序遍歷 (Level-order Traversal) 解法。

from collections import deque

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
            
        queue = deque([root])
        while queue:
            current = queue.popleft()
            
            # 交換當前節點的左右子樹
            current.left, current.right = current.right, current.left
            
            # 將非空的子節點加入佇列，留待後續層級處理
            if current.left:
                queue.append(current.left)
            if current.right:
                queue.append(current.right)
                
        return root

---

## 正確性說明

- **不遺漏 (No Omission)**：DFS 或 BFS 會確實走訪整棵樹的每一個節點 (Node)，確保每個節點的子樹指標都被存取並交換。
- **不重複 (No Duplication)**：在樹狀資料結構中，沒有循環依賴 (Cycles)，因此每個節點只會被當作父節點的子節點走訪一次，不會發生重複翻轉導致抵銷的問題。
- **Python 特性防錯**：Python 的 `a, b = b, a` 賦值機制會在等號右邊先計算出一個 Tuple，然後再解包 (Unpack) 賦值給左邊。這避免了傳統其他語言中 `node.left = invert(node.right)` 執行後，原先的 `node.left` 參考遺失，導致後續 `node.right = invert(node.left)` 拿到錯誤資料的問題。

---

## 複雜度分析

- **時間複雜度 (Time Complexity)**：$O(N)$
  其中 $N$ 是二元樹中的節點總數。無論是遞迴還是迭代版本，演算法都需要精確地訪問樹中的每一個節點一次來進行交換操作。
  
- **空間複雜度 (Space Complexity)**：$O(H)$
  其中 $H$ 是二元樹的高度 (Height)。
  - **遞迴版**：空間開銷主要來自於系統呼叫堆疊 (Call Stack)。在最壞情況下（樹完全傾斜，變成一條鏈狀結構），高度 $H = N$，空間複雜度為 $O(N)$。在最佳情況下（完美平衡二元樹），高度 $H = \log N$，空間複雜度為 $O(\log N)$。
  - **迭代版 (BFS)**：空間開銷來自佇列 (Queue)。在最壞情況下（完美平衡二元樹的最底層），佇列中最多同時存在 $N/2$ 個節點，空間複雜度同樣為 $O(N)$。

---

## 常見誤區

1. **指標覆蓋問題 (Overwriting Pointers)**：
   若不熟悉 Python 的 Tuple 賦值，面試者常寫出以下錯誤代碼：
   root.left = self.invertTree(root.right)
   root.right = self.invertTree(root.left) # ❌ 此時的 root.left 已經是右子樹翻轉過來的結果了！
   *解法*：必須使用暫存變數 (Temporary Variable)，或直接使用 Python 的 Tuple Unpacking。

2. **忽略終止條件 (Missing Base Case)**：
   忘記寫 `if not root: return`，會導致遞迴到葉節點 (Leaf Node) 的子節點 (None) 時觸發 `AttributeError: 'NoneType' object has no attribute 'left'`。

3. **回傳錯誤的節點 (Returning the wrong node)**：
   在遞迴的最後忘記 `return root`，導致上層接收到 `None`，整棵樹的結構崩壞。

---

## 小結

這是一道驗證「二元樹遍歷 (Tree Traversal)」與「指標操作 (Pointer Manipulation)」基本功的絕佳題目。掌握這題後，建議可以延伸練習以下題目，來鞏固對樹結構遞迴的理解：
- LeetCode 100. Same Tree (判斷相同樹)
- LeetCode 101. Symmetric Tree (判斷對稱樹)
- LeetCode 104. Maximum Depth of Binary Tree (二元樹最大深度)

有趣的小軼事：這道題目曾因為 Homebrew 作者 Max Howell 在面試 Google 時未能解出而被拒絕，從而在工程師圈子裡名聲大噪，是面試前必刷的基礎神題。