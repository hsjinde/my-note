# LeetCode 100. Same Tree (判斷相同二元樹)

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
這是一道典型的二元樹遍歷問題，我們採用**深度優先搜尋 (DFS, Depth-First Search)** 的遞迴策略：
1. **同步遍歷**：同時走訪兩棵樹的相同位置節點。
2. **基本情況 (Base Case)**：
   - 若兩節點皆為空，代表目前分支結構相同，回傳 `True`。
   - 若其中一個為空或數值不同，代表結構或內容不一致，回傳 `False`。
3. **遞迴邏輯**：當前節點相同時，必須確保「左子樹相同」且「右子樹相同」（使用 `AND` 邏輯連結）。

### English Interview Plan
To determine if two binary trees are identical, we employ a **Recursive DFS** approach:
1. **Simultaneous Traversal**: We traverse both trees $p$ and $q$ at the same time, node by node.
2. **Base Cases**:
   - If both nodes are `None`, they are structurally identical at this leaf level, so we return `True`.
   - If only one node is `None`, or if the values of the nodes differ (`p.val != q.val`), we return `False` as the trees are not the same.
3. **Recursive Step**: If the current nodes are identical, we recursively check if the left subtrees are the same AND if the right subtrees are the same. This follows the **Short-circuit Evaluation** principle.

---

## 演算法詳解

當我們呼叫 `isSameTree(p, q)` 時，程式會像這樣運作：



1. **結構比對**：首先排除「空節點」的情況。
   - `p` 是空且 `q` 是空 $\rightarrow$ OK!
   - `p` 是空或 `q` 是空（其中一個非空）$\rightarrow$ 失敗。
2. **數值比對**：檢查 `p.val` 是否等於 `q.val`。
3. **向下傳遞**：
   - 比較 `p.left` 與 `q.left`。
   - 比較 `p.right` 與 `q.right`。
   - 只有當上述兩者皆回傳 `True`，當前層級才會回傳 `True`。

---

## 程式碼實作

以下為 Python 的精簡實作：
```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # 情況 1: 兩者皆為空 (Base Case - Success)
        if not p and not q:
            return True
        
        # 情況 2: 其中一個為空，或是數值不相等 (Base Case - Failure)
        if not p or not q or p.val != q.val:
            return False

        # 遞迴檢查左、右子樹
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```
---

## 正確性說明

### 如何確保「不重複」與「不遺漏」？
- **不遺漏**：遞迴會走訪二元樹的每一個節點直到葉節點 (Leaf Nodes)。
- **不重複**：每個節點配對僅會被比較一次，且比較的結果會直接回傳給父節點。
- **重複元素處理**：雖然樹中可能存在相同數值的節點，但因為我們是「同步位置」比對，所以不同位置的相同數值不會造成誤判。

---

## 複雜度分析

- **Time Complexity (時間複雜度)**: $O(\min(N, M))$
  其中 $N$ 與 $M$ 分別為兩棵樹的節點數。演算法會在遇到第一個不匹配的節點時停止。在最壞情況下（兩棵樹完全相同），我們需要走訪所有節點一次。
- **Space Complexity (空間複雜度)**: $O(\min(H1, H2))$
  空間開銷來自於遞迴產生的**呼叫堆疊 (Call Stack)**。其深度等於樹的高度 $H$。
  - 最壞情況（退化成鏈狀樹）：$O(N)$。
  - 最好情況（平衡二元樹）：$O(\log N)$。

---

## 常見誤區

1. **判斷邏輯不夠簡潔**：
   面試者常寫出多層嵌套的 `if-else`。練習將 `if not p or not q` 與 `p.val != q.val` 合併，能顯出程式碼的俐落感。
2. **忽略空樹情況**：
   如果沒考慮到兩棵樹初始即為空的情況，程式會報錯。
3. **Short-circuit 的重要性**：
   在遞迴時使用 `and`。如果左子樹已經不相同了，程式不應該再去計算右子樹。

---

## 小結

這題的核心在於**遞迴定義**：一棵樹相同，定義為「根相同」且「左子樹相同」且「右子樹相同」。

### 推薦延伸練習
- LeetCode 101. Symmetric Tree (對稱二元樹)
- LeetCode 572. Subtree of Another Tree (另一棵樹的子樹)
- LeetCode 226. Invert Binary Tree (反轉二元樹)