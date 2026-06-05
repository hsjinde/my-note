```python!
class Solution:
    def isValid(self, s: str) -> bool:
        look_up = {"(":")", '{':'}','[':']'}
        S = []
        for i in s:
            if i in look_up:
                S.append(i)
            else:
                if not S or look_up[S.pop()] != i: 
                    return False
        return not S
```
time complexity: O(n)
Space complexity: O(n)