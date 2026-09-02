# LeetCode 543. Diameter of Binary Tree (二元樹的直徑)

## 目錄
- [思路總覽 (Overview & Interview Plan)](#思路總覽-overview--interview-plan)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
- [正確性說明](#正確性說明)
- [複雜度分析](#複雜度分析)
- [常見誤區](#常見誤區)
- [小結](#小結)

## 思路總覽 (Overview & Interview Plan)

**中文解題計畫：**
1. **核心概念**：二元樹的直徑不一定會經過根節點 (Root)，但必定會經過某個節點，且為該節點的「左子樹最大深度」加上「右子樹最大深度」。
2. **演算法選擇**：使用由下而上的深度優先搜尋 (Bottom-up DFS)。
3. **具體實作步驟**：
   - **Base Case**：當遇到空節點 (Null node) 時，深度為 0。
   - **遞迴邏輯**：分別計算左子樹與右子樹的最大深度。
   - **狀態更新**：在每個節點，計算經過該節點的路徑長度（左深度 + 右深度），並用此值更新全域的最大直徑 (Global maximum diameter)。
   - **回傳值**：向上層父節點回傳當前節點的最大深度（`max(左深度, 右深度) + 1`）。

**English Interview Plan:**
1. **Core Concept**: The diameter of a binary tree might not pass through the root. However, the longest path passing through any specific node is the sum of the maximum depths of its left and right subtrees.
2. **Algorithm Choice**: We will use a Bottom-up Depth-First Search (DFS).
3. **Execution Steps**:
   - **Base Case**: If the node is null, return a depth of 0.
   - **Recursive Logic**: Recursively compute the maximum depth of the left and right subtrees.
   - **State Update**: At each node, calculate the path length passing through it (`left_depth + right_depth`) and update the maximum diameter found so far.
   - **Return Value**: Return the maximum depth of the current subtree (`max(left_depth, right_depth) + 1`) to its parent.

## 演算法詳解

我們可以使用簡單的遞迴樹來描述狀態變化。假設有一棵樹：
      1
     / \
    2   3
   / \
  4   5

遞迴過程如下：
- 節點 4 (葉節點)：左深 0，右深 0。更新直徑 max(0, 0+0) = 0。回傳深度 max(0, 0) + 1 = 1。
- 節點 5 (葉節點)：左深 0，右深 0。更新直徑 max(0, 0+0) = 0。回傳深度 max(0, 0) + 1 = 1。
- 節點 2：左深 1 (來自 4)，右深 1 (來自 5)。更新直徑 max(0, 1+1) = 2。回傳深度 max(1, 1) + 1 = 2。
- 節點 3 (葉節點)：回傳深度 1。
- 節點 1 (根節點)：左深 2 (來自 2)，右深 1 (來自 3)。更新直徑 max(2, 2+1) = 3。回傳深度 max(2, 1) + 1 = 3。
最終最大直徑為 3。

## 程式碼實作

以下提供使用內部函式 (Nested function) 的寫法。這種寫法在 Python 中特別受面試官青睞，因為不需要維護類別級別 (Class-level) 的狀態，避免了多次呼叫 API 時變數未重置的問題。
```python
    class Solution:
        def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
            self.max_diameter = 0
            
            def max_depth(node: Optional[TreeNode]) -> int:
                # Base Case: 空節點深度為 0
                if not node:
                    return 0
                
                # 遞迴取得左右子樹的最大深度
                left_depth = max_depth(node.left)
                right_depth = max_depth(node.right)
                
                # 更新全域最大直徑 (左深度 + 右深度 = 經過此節點的邊數)
                self.max_diameter = max(self.max_diameter, left_depth + right_depth)
                
                # 回傳當前節點的最大深度給父節點
                return max(left_depth, right_depth) + 1
                
            max_depth(root)
            return self.max_diameter
```
## 正確性說明

- **不遺漏**：演算法透過 DFS 走訪了樹中的「每一個節點」。由於任何一條路徑必定有一個「最高點」（最靠近根節點的節點），在計算該最高點時，必定會囊括該路徑的長度。
- **不重複**：每次更新 `max_diameter` 都是獨立比較當前節點的最長路徑，只保留歷史最大值。
- **邊界條件**：當 `root` 為空時，`max_depth` 直接回傳 0，`max_diameter` 保持 0，符合預期。

## 複雜度分析

- **Time Complexity**: O(N)
  其中 N 是二元樹中的節點總數。我們對每個節點都恰好訪問一次（Post-order traversal）。
- **Space Complexity**: O(H)
  其中 H 是二元樹的高度。空間開銷主要來自於遞迴呼叫的系統堆疊 (Call stack)。
  - 最佳情況（完全平衡二元樹）：O(log N)
  - 最差情況（退化為鏈結串列）：O(N)

## 常見誤區

1. **混淆「節點數」與「邊數」**：題目要求的直徑是「邊 (Edges) 的數量」，而非節點數量。左右子樹的深度相加（`left_depth + right_depth`）剛好就是跨越當前節點的邊數，不需要額外減一。
2. **以為直徑一定經過根節點**：這是最常見的盲點。有些樹的某個子樹極度茂密，其內部的直徑可能大於穿過根節點的直徑。這就是為什麼我們必須在「每個節點」都更新一次最大值。
3. **變數狀態未重置**：如果像你原本的程式碼那樣使用 `self.dia`，在真實的面試或生產環境中，同一個 Solution 實例如果被多次呼叫 `diameterOfBinaryTree`，`self.dia` 不會歸零，會導致後續結果錯誤。建議在主函式入口處初始化變數（如我提供的程式碼所示）。

## 小結

這是一道非常經典的「由下而上深度優先搜尋 (Bottom-up DFS)」題型。核心觀念在於理解「回傳值（提供給父節點的深度）」與「副作用（更新全域直徑）」的區別。
- **延伸練習方向**：推薦接續練習 LeetCode 124. Binary Tree Maximum Path Sum，這題是本題的進階版，從計算「邊數」改為計算「節點權重總和」，並且需要處理負數權重的分支修剪 (Pruning)。