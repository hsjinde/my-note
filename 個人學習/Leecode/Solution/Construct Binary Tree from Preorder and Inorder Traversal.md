# 基本概念

![20111557YgB20xzqR3](https://hackmd.io/_uploads/BJGGyi50a.jpg)

1. 前序遍歷：順序是根節點、左子節點、右子節點，根排在前面。
[1, 2, 4, 7, 8, 5, 3, 6, 9, 10]
2. 中序遍歷：順序是左子節點、根節點、右子節點，根排在中間。
[7, 4, 8, 2, 5, 1, 3, 9, 6, 10]
3. 後序遍歷：順序是左子節點、右子節點、根節點，根排在後面。
[7, 8, 4, 5, 2, 9, 10, 6, 3, 1]
4. 層序遍歷：順序是由根節點一層一層往下，由左往右。
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 
```python!
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None

        rootValue = preorder[0]
        root = TreeNode(rootValue)
        inorderIndex   = inorder.index(rootValue)

        root.left = self.buildTree(preorder[1: inorderIndex + 1], inorder[:inorderIndex])
        root.right = self.buildTree(preorder[inorderIndex + 1:], inorder[inorderIndex+1:])
        return root
```
Time Complexity: O($n^2$)，The worst case occurs when the tree is left-skewed
*在每個節點上，都會執行 inorder.index(rootValue)，這個操作的時間複雜度是 O(n)。
*每個節點都會被訪問一次。

Space Complexity: O(n) ，The extra space used is due to the recursive call stack and the worst case occurs for a skewed tree
* 最壞情況下，樹是完全不平衡的，高度為 n，因此空間複雜度是 O(n)。
* 最佳情況下，樹是平衡的，高度為 log(n)，空間複雜度是 O(log(n))。