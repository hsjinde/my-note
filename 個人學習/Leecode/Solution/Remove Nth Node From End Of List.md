# Two Pointer Approach

題目是要刪除倒數第$n$個節點

因此我們設置一個$left$與一個$right$，並將$right$先移動$n$格，只需要同時移動$left$和$right$，最後當$right$指到$None$時，$left$的位置即是倒數第n個節點。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0,head)
        left = right = head
        for _ in range(n):
            right = right.next
        while right:
            left = left.next
            right = right.next
        left.next = left.next.next
        return dummy.next
```

The time complexity of this approach is O(n) as the code traverses the list once using two pointers (fast and slow) until the fast reaches the end. Here n is the number of nodes in the linked list.

The space complexity is O(1) as we haven’t used any extra space.
