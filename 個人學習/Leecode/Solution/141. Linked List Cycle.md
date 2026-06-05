# LeetCode 141. Linked List Cycle (環形鏈結串列)

## 目錄
- [思路總覽 (Overview & Interview Plan)](#思路總覽-overview--interview-plan)
- [演算法詳解](#演算法詳解)
- [程式碼實作](#程式碼實作)
- [正確性說明](#正確性說明)
- [複雜度分析](#複雜度分析)
- [常見誤區](#常見誤區)
- [小結](#小結)

---

## 思路總覽 (Overview & Interview Plan)

### 中文解題計畫
在面試中遇到鏈結串列 (Linked List) 是否有環的偵測問題，最直觀的解法是使用雜湊表 (Hash Set) 記錄走過的所有節點，但這會消耗 O(N) 的額外空間。為了展現對資料結構與演算法的深度掌握，我們應採用空間複雜度為 O(1) 的「快慢指標 (Fast and Slow Pointers)」技巧，這也是著名的弗洛伊德龜兔賽跑演算法 (Floyd's Tortoise and Hare Algorithm)。
具體實作步驟如下：
1. 預處理 (Initialization)：將快指標 (fast) 與慢指標 (slow) 同時初始化在鏈結串列的頭節點 (head)。
2. 核心遞迴/走訪邏輯 (Traversal Logic)：在一個迴圈中，慢指標每次前進一步 (`slow = slow.next`)，快指標每次前進兩步 (`fast = fast.next.next`)。
3. 狀態判斷與終止 (Base Case & Termination)：
   - 若存在環：由於快指標移動速度是慢指標的兩倍，快指標最終一定會在環內「套圈」並追上慢指標 (`slow == fast`)，此時回傳 True。
   - 若不存在環：快指標會順利走到鏈結串列的尾端 (`fast` 或 `fast.next` 為 null)，此時結束迴圈並回傳 False。

### English Interview Plan
To determine if a linked list has a cycle, the most optimal approach in terms of space complexity is Floyd's Cycle-Finding Algorithm, commonly known as the Fast and Slow Pointers technique. 
- Initialization: We start by initializing two pointers, `slow` and `fast`, both pointing to the `head` of the linked list.
- Core Logic (Traversal): We iterate through the list. In each iteration, the `slow` pointer moves exactly one step, while the `fast` pointer moves two steps.
- Termination & Base Case: If there is no cycle, the `fast` pointer will eventually reach the end of the list (where `fast` or `fast.next` becomes null), allowing us to safely return `False`. However, if a cycle exists, the `fast` pointer will loop around and eventually meet the `slow` pointer inside the cycle (`slow == fast`), at which point we return `True`. This guarantees an $O(1)$ space complexity without the need for an external Hash Set for tracking visited nodes.

---

## 演算法詳解


想像兩位跑者在跑道上賽跑。如果跑道是直線的（無環），跑得快的人一定會先抵達終點；但如果跑道是一個操場（有環），跑得快的人不僅不會遇到終點，還會不斷繞圈，最終從背後追上跑得慢的人。

演算法狀態變化如下：
1. 初始狀態：
   Slow -> Node 0
   Fast -> Node 0
2. 走訪過程：
   Slow 每次移動 1 步。
   Fast 每次移動 2 步。
3. 追及問題：
   假設兩指標都已進入環中。Fast 指標相對於 Slow 指標的速度是「每次移動靠近 1 步」。
   這意味著，無論環有多大，Fast 指標一定會以每次 1 步的相對速度穩穩地縮小與 Slow 之間的距離，直到兩者相遇，絕對不會發生「Fast 直接跳過 Slow」的錯過情況。

---

## 程式碼實作

以下為時間與空間皆為最佳化的 Python 實作版本：
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        # 預處理：快慢指標皆指向頭部
        slow = fast = head
        
        # 確保 fast 與 fast.next 存在，避免 Null Pointer Exception
        while fast and fast.next:
            slow = slow.next           # 慢指標走一步
            fast = fast.next.next      # 快指標走兩步

            # 若兩指標相遇，代表有環
            if slow == fast:
                return True
                
        # 快指標遇到盡頭，代表無環
        return False
```
---

## 正確性說明

- 不重複與不遺漏：快慢指標的相對速度為 1。當慢指標進入環時，快指標必然已經在環內。此時兩者距離最大為環的長度 L。因為相對速度為 1，最多經過 L 次迭代，快指標必然會精準落在慢指標所在的節點上，不會產生跳躍遺漏。
- 邊界條件處理 (Boundary Conditions)：
  - `head` 為空 (`None`)：`while fast and fast.next` 會直接判斷為 False，安全回傳 `False`。
  - 只有單一節點且無環：`fast.next` 為空，不會進入迴圈，回傳 `False`。
  - 只有單一節點且有環 (自己指向自己)：迴圈第一次執行後 `slow` 與 `fast` 都指向同一個節點，觸發 `slow == fast` 回傳 `True`。

---

## 複雜度分析

- Time Complexity (時間複雜度): O(N)
  - 無環情況：快指標走完 N 個節點只需 N/2 次迭代，時間複雜度為 O(N)。
  - 有環情況：假設非環部分長度為 K，環長度為 L，總節點數 N = K + L。慢指標走到環入口需要 K 步，此時快慢指標距離最大為 L。快指標追上慢指標最多需要 L 步。總迭代次數為 K + L = N，因此最壞時間複雜度為 O(N)。
- Space Complexity (空間複雜度): O(1)
  - 我們僅使用了兩個指標變數 `slow` 與 `fast`，不論鏈結串列多長，所佔用的記憶體空間皆為常數級別。

---

## 常見誤區

1. 誤用 Hash Set 導致空間浪費：雖然用 `set` 記錄 `visited` 節點邏輯直觀，但在面試中若沒有主動提出 O(1) 空間的解法，通常會被視為對指標技巧不夠熟悉。
2. 終止條件判斷錯誤：在 `while` 迴圈中，常常有初學者只寫 `while fast:`，忘記快指標一次要走兩步，如果 `fast.next` 是 `None`，執行 `fast.next.next` 時就會觸發 `AttributeError: 'NoneType' object has no attribute 'next'`。因此必須嚴格檢查 `while fast and fast.next:`。
3. 指標提早比較：若在 `slow = head`, `fast = head` 初始化後，尚未移動前就先在迴圈外或開頭比較 `if slow == fast`，會導致一開始就誤判，必須先移動再比較。

---

## 小結

- 核心觀念：本題是「快慢指標 (Fast and Slow Pointers)」的經典入門題，利用相對速度解決環形結構的偵測問題，完美示範了空間複雜度由 O(N) 降至 O(1) 的優化過程。
- 延伸練習方向：
  - LeetCode 142. Linked List Cycle II：不僅要判斷是否有環，還要找出「環的起點」。(需要利用數學推導出相遇點與起點的距離關係)
  - LeetCode 876. Middle of the Linked List：利用同樣的快慢指標技巧，在一次走訪內找到鏈結串列的中點。
  - LeetCode 287. Find the Duplicate Number：將陣列視為鏈結串列並運用環形偵測尋找重複數字。