利用[Merge Two Sorted Lists](/4ylzUX9-TySM4aVTLGuh2w)的方法，將lists中的LinkList倆倆合併。


# LeetCode 23. Merge k Sorted Lists  
(合併 K 個排序連結串列)

## 目錄
- [題目描述 (中文摘要)](#題目描述-中文摘要)
- [思路總覽](#思路總覽)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
  - [分治成對合併（與您程式一致）](#分治成對合併與您程式一致)
  - [小根堆版本（優先佇列）](#小根堆版本優先佇列)
- [正確性說明](#正確性說明)
- [時間複雜度分析](#時間複雜度分析)
- [空間複雜度分析](#空間複雜度分析)
- [常見誤區與補充](#常見誤區與補充)
- [延伸思考](#延伸思考)
- [小結](#小結)

---

## 題目描述 (中文摘要)
給定 `k` 個已排序（遞增）的單向連結串列，請將它們合併為一個排序後的連結串列並回傳其頭節點。

---

## 思路總覽
- 兩種常見有效解法（皆為 O(N log k)，N 為所有節點總數）：
  1. 分治成對合併：每輪將串列兩兩合併，長度減半，直到只剩一條。
  2. 小根堆：同時將每條串列的頭節點放入堆，每次彈出最小節點並推入其下一節點。

---

## 演算法詳解
1. 分治成對合併
   - 內部用「合併兩條已排序串列」的標準技巧（雙指標）。
   - 外部用 while 迴圈，每次走步長 2 將相鄰兩條合併，形成新陣列，直到只剩一條。

2. 小根堆
   - 用堆存放 (節點值, 串列索引, 節點)（索引避免值相等時比較節點物件）。
   - 每次彈出最小值節點，接到結果串列尾端，若該節點有 next 再推入堆。

---

## 程式碼實作

### 分治成對合併（與您程式一致）
```python
from typing import List, Optional

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None

        def merge2List(list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
            cur = head = ListNode()
            while list1 and list2:
                if list1.val < list2.val:
                    cur.next = list1
                    list1 = list1.next
                else:
                    cur.next = list2
                    list2 = list2.next
                cur = cur.next
            cur.next = list1 or list2
            return head.next

        # 可選：先過濾掉 None，減少不必要合併
        lists = [node for node in lists if node]

        if not lists:
            return None

        while len(lists) > 1:
            merged: List[Optional[ListNode]] = []
            for i in range(0, len(lists), 2):
                list1 = lists[i]
                list2 = lists[i + 1] if i + 1 < len(lists) else None
                merged.append(merge2List(list1, list2))
            lists = merged
        return lists[0]
```

### 小根堆版本（優先佇列）
```python
from typing import List, Optional, Tuple
import heapq

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        heap: List[Tuple[int, int, ListNode]] = []
        # 初始化堆
        for i, node in enumerate(lists):
            if node:
                heapq.heappush(heap, (node.val, i, node))

        dummy = cur = ListNode()
        while heap:
            val, i, node = heapq.heappop(heap)
            cur.next = node
            cur = cur.next
            if node.next:
                heapq.heappush(heap, (node.next.val, i, node.next))
        return dummy.next
```

---

## 正確性說明
- 合併兩串列步驟維持穩定排序，結果仍為排序串列。
- 分治將 k 條不斷兩兩合併，等價於多路合併，順序不變且最終全域有序。
- 小根堆版本每次取出目前最小節點，並在其來源串列中推入下一節點，保證全域最小性與單調遞增。

---

## 時間複雜度分析
- 分治成對合併：每一節點參與合併約 O(log k) 次，總時間 O(N log k)。
- 小根堆：每個節點做一次 heappush/heappop，單步 O(log k)，總時間 O(N log k)。

## 空間複雜度分析
- 分治成對合併：額外指標與暫存 O(1)（不計輸出）。
- 小根堆：堆大小最多 k，額外空間 O(k)。

---

## 常見誤區與補充
1. 忘記處理空陣列或全為 `None` 的情況。
2. 合併兩串列時漏掉尾端掛接 `cur.next = list1 or list2`。
3. 堆版本若不提供索引輔助，值相等時會嘗試比較節點物件而報錯。
4. 建議過濾 `None` 以減少合併次數，微幅優化。

---

## 延伸思考
- 若串列長度差異大，堆版本可能在常數上表現較佳。
- 可將分治改為遞迴自頂向下（同樣 O(N log k)），但需注意遞迴深度。

---

## 小結
- 兩種主流解皆為 O(N log k) 最優級別。
- 您的實作屬於「分治成對合併」，簡潔且高效；在工程中也常見小根堆作法。