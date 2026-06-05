[參考](https://www.cnblogs.com/grandyang/p/4640572.html)

```python!
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None 
        if root.val > max(p.val, q.val):
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < min(p.val, q.val):
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```

#### Time complexity: O(h)
h is the height of the tree.
In the worst case, when the tree is skewed (completely unbalanced), the time complexity is O(n), where n is the number of nodes in the tree.
#### Space complexity: O(1)
The algorithm uses only a constant amount of extra space.