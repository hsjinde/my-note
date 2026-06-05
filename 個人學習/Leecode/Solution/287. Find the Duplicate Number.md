# LeetCode 287. Find the Duplicate Number (尋找重複的數字)

## 標題與目錄
- [LeetCode 287. Find the Duplicate Number (尋找重複的數字)](#leetcode-287-find-the-duplicate-number-尋找重複的數字)
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
在面試中遇到這道題，通常考官會逐漸加上限制條件（例如：不能修改原陣列、只能使用常數級別的額外空間）。
1. **直覺解法 (Naive Approach)**：如同你提供的程式碼，最直覺的方式是建立一個「已訪問」的集合或陣列（Seen Array/List），遍歷原陣列並記錄。若發現數字已存在，即為重複值。但要注意，使用 List 進行尋找（`in seem`）的時間開銷極大，應改用 Hash Set 來優化尋找時間。
2. **最佳化解法 (Optimal Approach)**：為了達成題目進階要求的 O(1) 空間複雜度，我們必須將陣列視為一個「鏈結串列 (Linked List)」。因為陣列內的值在 1 到 n 之間，我們可以將「陣列的值」當作「下一個節點的索引 (Index)」。既然有重複的數字，代表有多個索引會指向同一個值，這必然會形成「循環 (Cycle)」。因此，我們可以使用「快慢指標 (Fast and Slow Pointers)」也就是 Floyd 判圈演算法來找出循環的起點，該起點即為重複的數字。

### English Interview Plan
1. **Initial Brute Force / Hash Set**: I will start by mentioning the straightforward approach. We can iterate through the array and store seen elements in a Hash Set. If an element is already in the Set, we return it. While this guarantees O(N) time complexity, it requires O(N) auxiliary space, which doesn't strictly meet the most advanced problem constraints.
2. **Cycle Detection (Floyd's Tortoise and Hare)**: To optimize the space complexity to O(1) without modifying the array, I will model the array as a Linked List. The indices represent the current nodes, and the values represent the `next` pointers. Because multiple nodes point to the same duplicated value, a cycle is guaranteed to exist. I will use a slow pointer (moves 1 step) and a fast pointer (moves 2 steps) to detect the intersection point, and then reset one pointer to the start to find the cycle's entrance, which corresponds to the duplicate number.

---

## 演算法詳解

針對你原先的邏輯與面試中期望的最佳解法，我們進行拆解：

1. **陣列/集合記錄法 (你的原始思路)**
   - 建立一個空的容器 `seem`。
   - 逐一檢查陣列中的元素 `nums[i]`。
   - 若 `nums[i]` 已經在 `seem` 中，說明找到重複值，直接回傳。
   - 若不在，則將其加入 `seem` 中。
   - **優化點**：你的原碼使用 List (`seem = []`)，這會導致每次檢查 `in seem` 都需要遍歷整個 List。將其改為 Set (`seem = set()`) 可以將查找時間降為常數時間。

2. **快慢指標判圈法 (進階面試標準解)**
      - **建立圖論模型**：將索引 `i` 視為節點，`nums[i]` 視為指向下一個節點的邊。例如：`0 -> nums[0] -> nums[nums[0]]`。
   - **第一階段 (尋找相遇點)**：
     - 初始化慢指標 `slow` 與快指標 `fast`，皆從起點 `nums[0]` 出發。
     - `slow` 每次走一步（`slow = nums[slow]`）。
     - `fast` 每次走兩步（`fast = nums[nums[fast]]`）。
     - 由於存在重複數字（即循環），快指標必定會在迴圈內追上慢指標。
   - **第二階段 (尋找循環起點/重複數字)**：
     - 當 `slow` 與 `fast` 相遇時，將其中一個指標（例如 `slow`）重新移回起點 `nums[0]`。
     - 這次兩個指標都每次只走一步。
     - 當它們再次相遇時，該相遇點即為「循環的起點」，也就是我們要求的那顆重複的數字。

---

## 程式碼實作

原始版本（將 List 轉為 Set 優化查詢效率）：
```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # 將原本的 List 改為 Set (Hash Set)，以達到 O(1) 的查詢效率
        seen = set()
        for num in nums:
            if num in seen:
                return num
            seen.add(num)
```

進階最佳化版本（快慢指標，O(1) 空間）：
```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # 階段 1: 尋找相遇點 (Intersection point)
        slow = nums[0]
        fast = nums[0]
        
        while True:
            slow = nums[slow]           # 走一步
            fast = nums[nums[fast]]     # 走兩步
            if slow == fast:
                break
                
        # 階段 2: 尋找循環起點 (Entrance to the cycle)
        slow = nums[0]                  # 將 slow 移回起點
        while slow != fast:
            slow = nums[slow]           # 兩者皆走一步
            fast = nums[fast]
            
        return slow                     # 相遇點即為重複的數字
```
---

## 正確性說明

- **不遺漏與必定存在**：根據「鴿巢原理 (Pigeonhole Principle)」，$n+1$ 個數字放入範圍 $1$ 到 $n$ 的區間中，必定至少有一個數字重複。
- **循環必定產生**：因為陣列值範圍在 $[1, n]$，所以 `nums[i]` 必定是一個有效的陣列索引。重複的數字代表有兩個以上的索引指向同一個位置，這在圖論中就是一個「多重入度」的節點，必然會形成一個循環。
- **第二階段相遇點證明**：設起點到循環起點距離為 $a$，循環起點到相遇點距離為 $b$，循環剩餘長度為 $c$。相遇時，慢指標走了 $a + b$，快指標走了 $a + b + k(b+c)$。因為快指標速度是慢指標兩倍，所以 $2(a + b) = a + b + k(b+c)$，推導得出 $a = c + (k-1)(b+c)$。這意味著，從起點走 $a$ 步，與從相遇點走 $c$ 步（加上繞幾圈）最終會停在同一個點，也就是循環起點。

---

## 複雜度分析

以最佳化的「快慢指標法」為基準：
- **Time Complexity**: O(N)
  - 在第一階段，快慢指標最多只會繞行陣列（或循環）常數圈即相遇，耗時與陣列長度成正比。
  - 第二階段，尋找起點也最多只會遍歷陣列一次。總合時間複雜度為線性時間 O(N)。
  - （註：你原先的 List 尋找版本，因為 `in seem` 是 O(N) 操作，在 for 迴圈內會導致總體時間複雜度劣化為 O(N^2)）。
- **Space Complexity**: O(1)
  - 快慢指標法只使用了兩個指標變數 (`slow`, `fast`)，沒有建立額外的資料結構（如 Set 或 List），完全符合題目最嚴格的常數空間要求。

---

## 常見誤區

1. **誤用 List 進行尋找**：正如你原本的程式碼，使用 `List` 來進行 `in` 的包含檢查。這會使每一次查找都需要 O(N) 時間，對於較大測資會直接發生 Time Limit Exceeded (TLE)。應養成習慣使用 Hash Set 來做去重與尋找。
2. **改變原陣列 (Modifying the array)**：有些面試者會直覺想到先將陣列排序（`nums.sort()`），然後檢查相鄰元素。雖然空間是 O(1)，但排序改變了原陣列，且時間複雜度為 O(N log N)，違反了進階限制。
3. **錯誤設定指標起點**：在使用快慢指標時，必須確保 `slow` 和 `fast` 都是從 `nums[0]` 出發。若直接用 `slow = 0`, `fast = 1` 開始，會破壞鏈結串列尋找循環起點的數學推導，導致第二階段找不到正確的重複數字。

---

## 小結

這道題是將「陣列」轉換為「鏈結串列 (Linked List)」並結合「Floyd 判圈演算法」的經典神題。你的初步思路非常好，但在實務與面試中，資料結構的選擇（List vs Set）對於效能有決定性的影響。建議熟練掌握快慢指標的運用，這在處理 Linked List 循環問題（如 LeetCode 141. Linked List Cycle、142. Linked List Cycle II）時都會是共通的解題核心觀念！