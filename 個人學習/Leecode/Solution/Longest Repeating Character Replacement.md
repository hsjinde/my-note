給定string $s$ 和 integer $k$，可以最多置換$k$個字母，希望可以得到最長的連續字母。
```pyhon=
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        maxlen, longestCount = 0,0
        hashmap = collections.Counter()
        for i in range(len(s)):
            hashmap[s[i]] += 1
            longestCount = max(longestCount, hashmap[s[i]] )
            if maxlen - longestCount >= k:
                hashmap[s[i - maxlen]] -= 1
            else:
                maxlen += 1
        return maxlen
```

令s = "AABABBA", k = 1


| 比對狀況    | Counter                   | maxlen | largestCount |
|:----------- |:------------------------- |:------:|:------------:|
| AABABBA     | Counter()                 |   0    |      0       |
| ==A==ABABBA | Counter({'A': 1})         |   1    |      1       |
| ==AA==BABBA | Counter({'A': 2})         |   2    |      2       |
| ==AAB==ABBA | Counter({'A': 2, 'B': 1}) |   3    |      2       |
| ==AABA==BBA | Counter({'A': 3, 'B': 1}) |   4    |      3       |
| A==ABAB==BA | Counter({'A': 2, 'B': 2}) |   4    |      3       |
| AA==BABB==A | Counter({'A': 1, 'B': 3}) |   4    |      3       |
| AAB==ABBA== | Counter({'A': 2, 'B': 2}) |   4    |      3       |

1. maxlen 變數用於記錄目前找到的最長連續子串的長度，longestCount 變數用於記錄曾經出現過的最多字符的次數。
2. hashmap 是一個計數器，用於記錄每個字符出現的次數。
3. 接下來，我們將遍歷輸入字符串 s 的每個字符。
4. 對於每個字符，我們將其添加到 hashmap 中並更新 longestCount。
5. 然後檢查當前的最大子串長度是否超過了 longestCount（即最常出現的字符的次數），如果超過了，我們需要開始移動窗口，即減少左邊界的位置以滿足條件。
6. 我們只有在最長子串的長度減去最常出現字符的次數小於等於 k 時才增加 maxlen。這是因為我們要找到可以替換的最多字符數量 k，以便獲得最長的連續子串。
7. 當遍歷完整個字符串後，maxlen 就是我們想要的結果，即替換最多 k 個字符的情況下，可以獲得的最長連續子串的長度。















**有一點Sliding Window的概念**

Time Complexity :  O(n)
Space Complexity : O(1)
[參考](https://ithelp.ithome.com.tw/m/articles/10288169)