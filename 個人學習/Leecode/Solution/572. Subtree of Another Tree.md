## 標題與目錄
- LeetCode 572. Subtree of Another Tree (另一棵樹的子樹)

### 目錄
- [思路總覽 (Overview & Interview Plan)](#思路總覽-overview--interview-plan)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
- [正確性說明](#正確性說明)
- [複雜度分析](#複雜度分析)
- [常見誤區](#常見誤區)
- [小結](#小結)

---

## 思路總覽 (Overview & Interview Plan)

**中文解題計畫：**
這道題目的核心在於「樹的遍歷」與「樹的比對」。在面試中，我會採取「雙重遞迴 (Double DFS)」的策略來破解：
1. **主體遍歷 (外層遞迴)：** 將 `root` 樹上的每一個節點都視為一個「潛在的子樹根節點」。如果當前節點為空，代表找不到，直接回傳 `False`。
2. **結構比對 (內層遞迴)：** 對於每一個遍歷到的潛在根節點，呼叫一個輔助函式 `isSameTree`，同步往下比對它與 `subRoot` 的每一個節點值與結構。
3. **分治邏輯：** 如果當前節點比對成功，回傳 `True`；若失敗，則繼續向左子樹或右子樹尋找（使用 `or` 邏輯連結）。

**English Interview Plan:**
To determine if `subRoot` is a subtree of `root`, I will implement a **Double-DFS (Depth-First Search)** strategy. 
- The outer DFS will traverse the main `root` tree. At each node, we treat it as a candidate root and trigger the inner DFS.
- The inner DFS (`isSameTree`) acts as a structural and value equivalence checker. It simultaneously traverses both the candidate subtree and `subRoot`.
- We must strictly define our **Base Cases**: if the main tree node is null, we return false. If both trees in the inner DFS reach a null state simultaneously, they match. 
- To optimize, we rely on **Short-circuit Evaluation** (using logical OR) to halt the search early once a matching subtree is identified. While this takes O(M * N) time, I can also discuss advanced approaches like **Tree Serialization combined with KMP** or **Merkle Hashing** for an O(M + N) time complexity if performance becomes a bottleneck.

---

## 演算法詳解

這個解法本質上是在一棵大樹中，不斷地套用「兩棵樹是否相同」的邏輯。

**遞迴狀態變化：**
假設 `root` 為 [3, 4, 5, 1, 2]，`subRoot` 為 [4, 1, 2]

      root               subRoot
       3                   4
      / \                 / \
     4   5               1   2
    / \
   1   2

1. 檢查 `root` (3)：`isSameTree(3, 4)` -> 值不同，失敗。
2. 切割問題：尋找 `root.left` (4) 或 `root.right` (5)。
3. 檢查 `root.left` (4)：`isSameTree(4, 4)` -> 進入內層遞迴。
   - 節點 4 相同，繼續比對左子樹 (1 與 1) 與右子樹 (2 與 2)。
   - 左子樹相同，右子樹相同。
   - `isSameTree` 回傳 `True`。
4. 因為左子樹找到目標，外層的 `or` 邏輯產生短路 (Short-circuit)，整體直接回傳 `True`。

---

## 程式碼實作

以下為您提供的優雅實作（標準 DFS 版本），變數命名清晰且邏輯嚴謹：

```python
    class Solution:
        def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
            # Base case: 尋找範圍已經為空，不可能包含 subRoot
            if not root:
                return False
        
            # 檢查以當前節點為根的樹，是否與 subRoot 完全相同
            if self.isSameTree(root, subRoot):
                return True
            
            # 若當前節點不同，繼續往左子樹或右子樹尋找
            return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
            
        def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
            # 兩者皆為空，結構與值皆吻合
            if not p and not q:
                return True
            # 其中一個為空，或兩者值不相等，代表不吻合
            if not p or not q or p.val != q.val:
                return False
            
            # 遞迴檢查左半邊與右半邊
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)

**優化思路探討 (Serialization + KMP)：**
若面試官要求更低的時間複雜度，可以將兩棵樹進行「前序遍歷序列化 (Preorder Serialization)」，並在空節點補上特殊符號（如 `#`），在節點前加上分隔符（如 `^`）。這樣問題就轉換成了「字串子字串搜尋問題」，可利用 KMP 演算法將時間複雜度降至 O(M + N)。
```
---

## 正確性說明

- **不遺漏：** `isSubtree` 中的 `root.left` 與 `root.right` 確保了原樹中的「每一個」節點都會被當作潛在的根節點去呼叫 `isSameTree`，因此絕對不會漏掉任何一個可能的位置。
- **不誤判：** `isSameTree` 嚴格要求結構與數值的一致性。特別是 `if not p and not q` 與 `if not p or not q` 的先後順序，完美處理了兩棵樹深度不同時的邊界條件，防止了「subRoot 只是 root 某部分前綴」的誤判情況。

---

## 複雜度分析

假設 `root` 的節點數為 M，`subRoot` 的節點數為 N。

- **Time Complexity: O(M * N)**
  外層遞迴 `isSubtree` 最壞情況下會遍歷 `root` 的每一個節點（共 M 次）。對於每一個節點，內層的 `isSameTree` 最壞情況下需要比對 `subRoot` 的每一個節點（共 N 次）。因此整體時間複雜度為 O(M * N)。
- **Space Complexity: O(H_m + H_n)**
  空間複雜度取決於遞迴呼叫堆疊 (Call Stack) 的最大深度。外層遞迴的最大深度為 `root` 的高度 H_m，內層遞迴的最大深度為 `subRoot` 的高度 H_n。在最壞情況下（樹呈現鏈狀退化），空間複雜度可能達到 O(M + N)。

---

## 常見誤區

1. **忽略空樹的 Base Case：** 忘記在 `isSubtree` 開頭寫 `if not root:`，導致遞迴到底層時出現 `AttributeError: 'NoneType' object has no attribute 'left'`。
2. **邏輯閘誤用：** 在 `isSubtree` 中誤將 `or` 寫成 `and`，或者在 `isSameTree` 中誤將 `and` 寫成 `or`。記住：尋找子樹是「只要有一處符合即可」(OR)，而判斷樹相同是「所有結構與值都必須吻合」(AND)。
3. **部分匹配的陷阱：** 如果 `isSameTree` 裡面沒有處理好 `subRoot` 提早結束的情況（例如 `subRoot` 遍歷完了，但 `root` 還有子節點），會導致誤判。您的寫法 `if not p or not q` 完美避開了這個問題。

---

## 小結

這道題目完美結合了**樹的遍歷 (Tree Traversal)** 與**分治法 (Divide and Conquer)**，是測試遞迴基本功的絕佳考題。熟練掌握 `isSameTree` 的寫法是非常高投資回報的，因為這個邏輯可以直接套用在其他如「對稱樹 (Symmetric Tree)」或「翻轉二元樹 (Invert Binary Tree)」的變體問題上。建議可以延伸練習 **LeetCode 100. Same Tree** 與 **LeetCode 101. Symmetric Tree** 來鞏固這套思維。