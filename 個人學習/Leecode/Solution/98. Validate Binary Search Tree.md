# Iterative
- 用Stack
    - 查看每一個點是不適BST
```python!
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return False
        lowest, highest = float('-inf'),float('inf')
        S = [[root, lowest, highest]]
        while S:
            root, lowest, highest = S.pop()

            if not root:
                continue 
            
            if root.val <= lowest or root.val >= highest:
                return False
            
            S.append([root.left, lowest, root.val])
            S.append([root.right, root.val, highest])
        return True
```

Time complexity: O(n), n is the input tree
Space complexity: O(n), size of the stack

# Recursive
```python!
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.valid(root, float('-inf'), float('inf'))
    def valid(self, node, lowest, highest):
        if not node:
            return True
        
        if node.val <= lowest or node.val >= highest:
            return False
        
        return self.valid(node.left, lowest, node.val) and self.valid(node.right, node.val, highest)
```

Time complexity: O(n)
Space complexity: O(n) 