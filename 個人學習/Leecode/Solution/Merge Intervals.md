當前面一個interval後面的值小於現在的Interval前面的值代表有overlap
因此比較前一個Interval後面的值與現在Interval後面的值(取最寬)
```python!
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        #先排序確保起始位置的順序
        intervals.sort()
        i =1 
        while i < len(intervals):
            #如果overlap就進行合併
            if intervals[i-1][1] >= intervals[i][0]:
                intervals[i-1][1] = max(intervals[i-1][1], intervals[i][1])
                intervals.pop(i)
            else:
                i += 1
        return intervals
```
Time complexity: O(nlogn)
Space complexity: O(n)