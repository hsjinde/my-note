暴力解，遍歷每一個字元，每次都以其作為中點向左右展開判斷是否還是迴文，是就加一。看程式碼就很直白了
```python
class Solution:
    def countSubstrings(self, s:str) -> int:
        res = 0
        for i in range(len(s)):
            l = r = i
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
            l = i
            r = i + 1
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
        return res
```

精簡版:
```python
class Solution:
    def countSubstrings(self, s:str) -> int:
        def countPali(s, l, r):
            res = 0
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
            return res 
        res = 0      
        for i in range(len(s)):
            res += countPali(s, i, i)
            res += countPali(s, i, i + 1)
        return res
```

Complexity
Time Complexity: $O(N^2)$
Space Complexity: $O(1)$