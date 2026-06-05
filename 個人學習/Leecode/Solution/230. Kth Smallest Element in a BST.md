應用Inorder的方法
1. 找到最左邊的子點
2. 將最左邊的子點加入$res$中
3. 確認該點是否為第$k$小
4. 嘗試將加入最左邊的右子點

```python=
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        self.res = []
        self.Inorder( root, k)
        return self.res[-1]
    def Inorder(self, root, k):
        if not root : return
        self.Inorder(root.left, k)
        if len(self.res) == k: return 
        self.res.append(root.val)
        self.Inorder(root.right, k)
```
#### Time complexity: The time complexity of this solution is $O(n)$
where n is the number of nodes in the binary search tree. The inorder traersal visits each node once.
(當需要遍歷大部分節點)

#### Space complexity: The space complexity is $O(h)$
where h is the height of the binary search tree. The space complexity is determined by the recursion stack.