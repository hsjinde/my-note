- [x] 對 intervals 根據 start 來做 sorting
- [x] 初使化 count = 0, overlapEnd = intervals[0][1]
- [x] 從 pos = 2 開始比較 intervals[pos][0] 是否小於 overlapEnd
- [x] 如果是, 代表有重疊 需要更新 count = count + 1, 更新 overlapEnd = min(intervals[pos][1], overlapEnd)
- [x] 如果否, 代表沒有重疊 繼續更新 overlapEnd = intervals[pos][1]
- [x] 當比完所有的區間 count 就是所求

```python!
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort()
        res = 0
        preEnd = intervals[0][1]

        for start, end in intervals[1:]:
            if start >= preEnd:
                preEnd = end
            else:
                res += 1
                preEnd = min (preEnd, end)
        return res
```
Time complexity: O(nlog n) -because of sorting
Space complexity: O(n) -because of sorting
[參考YT](https://www.youtube.com/watch?v=nONCGxWoUfM&ab_channel=NeetCode)

