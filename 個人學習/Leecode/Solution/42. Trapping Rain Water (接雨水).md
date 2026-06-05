# LeetCode 42. Trapping Rain Water (接雨水)

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

**中文解題計畫**
這是一道極具代表性的面試題，考察候選人對陣列與指標的掌握度。在面試中，我會遵循以下策略進行作答：
1. **破題與直覺**：首先點出每個位置能接的雨水量，取決於其「左側最高柱子」與「右側最高柱子」的較小值，減去自身的高度。
2. **演算法推進**：從空間複雜度 O(N) 的動態規劃 (Dynamic Programming) 解法（預先用陣列記錄左右最大值），自然過渡到空間複雜度 O(1) 的雙指標 (Two Pointers) 最佳化解法，展現遞進的優化思維。
3. **雙指標核心邏輯**：
   - 維護左右兩個指標 (`left`, `right`) 以及兩側的目前最高高度 (`l_max`, `r_max`)。
   - 比較 `height[left]` 與 `height[right]`，較矮的一側決定了「短板效應」 (木桶原理)，因此我們能安全地計算該側的蓄水量，並將指標向內收縮。

**English Interview Plan**
1. **Core Idea**: The water trapped at any bar is determined by the shorter side: min(l_max, r_max) - height[i].
2. **Optimization Strategy**: I will use the **Two Pointers** approach to optimize the space complexity to O(1).
3. **Execution Logic**:
    - Place pointers at both ends (left, right) and track max heights.
   - Always process the side with the **smaller height**, because it acts as the bottleneck.
   - Add water or update the max height, then move the pointer inward until they meet.
---

## 演算法詳解

演算法的核心在於**「誰小聽誰的」**。我們不需要真的知道整體的最高點在哪裡，只需要知道當前左右兩邊哪一邊是短板。

1. **初始化**：
   - `left` 設為 0，`right` 設為陣列末端 `len(height) - 1`。
   - `l_max` 與 `r_max` 初始化為 0。
   - 總水量 `total_trap` 初始化為 0。

2. **雙指標收縮 (Two Pointers Convergence)**：
   - 當 `left < right` 時，進入迴圈進行判斷。
   - **情況 A：`height[left] < height[right]`**
     - 這保證了對於 `left` 指標而言，右側存在一個高於或等於 `height[right]` 的「擋板」。因此，`left` 位置能裝多少水，**完全取決於左側的最大值 `l_max`**。
     - 如果 `height[left] > l_max`，代表左側沒有比當前更高的擋板，無法蓄水，此時更新 `l_max = height[left]`。
     - 否則（`height[left] <= l_max`），該位置必定能蓄積 `l_max - height[left]` 的水量。
     - 結算後，`left` 指標向右移動 (`left += 1`)。
   - **情況 B：`height[left] >= height[right]`**
     - 同理，對於 `right` 來說，左側有夠高的牆壁保障，蓄水上限完全取決於右側的最大值 `r_max`。
     - 如果 `height[right] > r_max`，更新 `r_max = height[right]`。
     - 否則，蓄積 `r_max - height[right]` 的水量。
     - 結算後，`right` 指標向左移動 (`right -= 1`)。

---

## 程式碼實作

以下為 O(1) 空間複雜度的雙指標最佳化版本實作：

```python
    class Solution:
        def trap(self, height: List[int]) -> int:
            total_trap = 0
            l_max, r_max = 0, 0
            left, right = 0, len(height) - 1

            while left < right:
                # 比較左右指標當前所在高度，決定哪一側是短板
                if height[left] < height[right]:
                    # 處理左側：更新最高點或計算蓄水
                    if height[left] > l_max:
                        l_max = height[left]
                    else:
                        total_trap += l_max - height[left]
                    # 左指標內縮
                    left += 1
                else:
                    # 處理右側：更新最高點或計算蓄水
                    if height[right] > r_max:
                        r_max = height[right]
                    else:
                        total_trap += r_max - height[right]
                    # 右指標內縮
                    right -= 1
                    
            return total_trap
```
---

## 正確性說明

- **不漏算**：每一次 `while` 迴圈必定會處理 `left` 或 `right` 其中一個位置的蓄水計算，直到兩個指標相遇，確保了陣列中每個凹槽的蓄水都會被獨立且完整地計算一次。
- **不重複/不超算 (Bottleneck 原理)**：為何 `height[left] < height[right]` 時只看 `l_max` 是安全的？因為就算未知的 `r_max` 比當前的 `height[right]` 還要巨大，對於 `left` 位置來說，水面的高度依然會被左側較矮的 `l_max` 限制住。我們確信右邊已經有足夠高的屏障，所以直接用 `l_max - height[left]` 計算絕對正確。
- **邊界條件 (Base Cases)**：如果陣列長度小於 3（例如長度為 2），迴圈雖然會執行，但因為一開始就會被更新為 `l_max` 或 `r_max` 而不會進入累加水的 `else` 分支，最終回傳 0，符合物理直覺。

---

## 複雜度分析

- **Time Complexity (時間複雜度)**: `O(N)`
  - 其中 N 是陣列 `height` 的長度。雙指標 `left` 與 `right` 分別從兩端向中間移動，陣列中的每個元素最多被訪問一次，因此總共執行 N 步操作，時間開銷為線性。
- **Space Complexity (空間複雜度)**: `O(1)`
  - 演算法執行過程中，僅額外使用了幾個常數級別的指標與計數變數 (`left`, `right`, `l_max`, `r_max`, `total_trap`)。相比於需要維護前綴最大值陣列的 DP 解法，不需要隨資料規模增長而消耗額外記憶體。

---

## 常見誤區

1. **條件判斷混淆**：
   - 誤以為比較的是 `l_max` 和 `r_max`，其實在雙指標推進策略中，我們比較的是 **`height[left]` 與 `height[right]`** 以確保哪一側有實際的較高擋板，從而確認「短板」。
2. **更新與計算順序錯誤**：
   - 有些人在實作時會先移動指標 (`left += 1`) 再去比較與計算，這會導致錯位計算 (Off-by-one Error)。必須確保「先結算當前指標位置的雨水，再移動指標」。
3. **過度複雜化邊界處理**：
   - 在陣列長度為極端值 (如 0 或 1) 時，初學者可能會寫一大堆 `if len(height) <= 2: return 0`。但這段雙指標邏輯在 `len(height) < 2` 時根本不會進入 `while` 的實質蓄水計算區塊，程式設計本身的魯棒性 (Robustness) 已經涵蓋了邊界。

---

## 小結

- 本題的核心觀念在於**木桶效應 (短板原理)**與**雙指標的動態逼近**。
- 能流暢地從 O(N) 空間複雜度的解法引導至 O(1) 的雙指標解法，是面試獲得 Strong Hire 的關鍵。
- **延伸練習方向**：建議一併練習 **LeetCode 11. Container With Most Water (盛最多水的容器)** 以及 **LeetCode 84. Largest Rectangle in Histogram (柱狀圖中最大的矩形)**，這兩題分別涵蓋了類似的雙指標貪心思維以及單調堆疊 (Monotonic Stack) 的技巧，是面積/蓄水類題目的黃金組合。