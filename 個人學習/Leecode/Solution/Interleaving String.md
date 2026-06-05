
s1 = aabcc
s2 = dbbca
s3 = aadbbcbcac

|       |    d     |    b     |    b     |    c     |    a     |    -     |
|:-----:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| **a** | ==True== |  False   |  False   |  False   |  False   |  False   |
| **a** | ==True== |  False   |  False   |  False   |  False   |  False   |
| **b** | ==True== | ==True== | ==True== | ==True== | ==True== |  False   |
| **c** |  False   | ==True== | ==True== |  False   | ==True== |  False   |
| **c** |  False   |  False   | ==True== | ==True== | ==True== | ==True== |
|   -   |  False   |  False   |  False   | ==True== |  False   | ==True== |



```python!
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if len(s1) + len(s2) != len(s3):
            return False
        
        dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
        
        for i in range(len(s1) + 1):
            for j in range(len(s2) + 1):
                if i == 0 and j == 0:
                    dp[i][j] = True
                elif i == 0:
                    dp[i][j] = dp[i][j - 1] and s2[j - 1] == s3[i + j - 1]
                elif j == 0:
                    dp[i][j] = dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]
                else:
                    dp[i][j] = (dp[i][j - 1] and s2[j - 1] == s3[i + j - 1]) or \
                               (dp[i - 1][j] and s1[i - 1] == s3[i + j - 1])
        
        return dp[len(s1)][len(s2)]

```


### Time complexity: $O(m*n)$
- The solution iterates over each possible (i,j)(i, j)(i,j) combination, leading to a time complexity of O(m×n)O(m \times n)O(m×n).
### Space complexity: $O(m*n)$
- The space complexity is O(m×n)O(m \times n)O(m×n) due to the 2D dpdpdp array.

# My plan

當 s1 和 s2 一樣 -> append to Q 

```python!
class Solution:
    @classmethod
    def isInterleave(cls, s1: str, s2: str, s3: str) -> bool:
        if len(s1) + len(s2) != len(s3):
            return False
        
        visited = set()  # 用集合來保存已經訪問過的(i, j)組合，避免重複計算
        stack = [(0, 0)]  # 初始化堆疊，從(s1[0], s2[0])開始
        
        while stack:
            i, j = stack.pop()  # 從堆疊中取出(i, j)
            
            if (i, j) in visited:
                continue  # 如果已經訪問過(i, j)，則跳過
            
            visited.add((i, j))  # 將(i, j)標記為已訪問
            
            # 如果已經匹配到了s3的最後一個字符，返回True
            if i + j == len(s3):
                return True
            
            # 判斷s1的下一個字符是否匹配s3的下一個字符，如果是，將(i+1, j)加入堆疊
            if i < len(s1) and s1[i] == s3[i + j]:
                stack.append((i + 1, j))
            
            # 判斷s2的下一個字符是否匹配s3的下一個字符，如果是，將(i, j+1)加入堆疊
            if j < len(s2) and s2[j] == s3[i + j]:
                stack.append((i, j + 1))
        
        return False

```