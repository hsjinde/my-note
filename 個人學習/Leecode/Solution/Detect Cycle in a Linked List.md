### 步驟 1.
快指針 fast 一次走兩步，
慢指針 slow 一次走一步，
所以快指針 fast 永遠會追擊著慢指針 slow，
期待有 cycle 的話， fast 會追上 slow ，
這裡我們先讓 fast = head.next; slow = head;。

### 步驟2.
這是主要的一步，
如果 Linked List 沒有 cycle 的話，那 fast 會遇到 null， return false 。
如果 Linked List 有 cycle 的話，那 fast 一定會遇到 slow，並且 fast 已經超過 slow 一整圈， return true。
完成！
```python!
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast = slow = head
        while fast and fast.next :
            if fast == slow:
                return True
            
            fast = fast.next.next
            slow = slow.next
        return False
```
Time complexity: O(n)
Space complexity: O(n)