# LeetCode 121. Best Time to Buy and Sell Stock (買賣股票的最佳時機)

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
這是一道經典的貪心演算法 (Greedy Algorithm) 與動態規劃 (Dynamic Programming) 的基礎題。我們不需要找出所有的買賣組合，只要在遍歷價格陣列的過程中，持續追蹤「歷史最低的購買價格」以及「當前能獲得的最大利潤」即可。
1. **初始化狀態**：設定初始最大利潤為 0，並將第一天的價格設為初始購買價格。
2. **單次遍歷 (One-pass)**：走訪每一天的價格（視為賣出價）。
3. **更新利潤或買價**：
   - 若當前價格大於購買價格，表示有利可圖，計算差價並更新最大利潤。
   - 若當前價格小於或等於購買價格，代表我們找到了一個更低的「潛在買入點」，因此更新購買價格。

### English Interview Plan
The optimal approach relies on a Single Pass (One-pass) strategy using the Greedy property.
1. **State Initialization**: Initialize `profit` to 0 and `buy` pointer to the first element in the array.
2. **Linear Scan**: Iterate through the `prices` array, treating each element as a potential `sell` price.
3. **State Transition**:
   - If `sell > buy`, we have a positive profit margin. We update the `profit` by taking the maximum of the current `profit` and `sell - buy`.
   - If `sell <= buy`, we have found a new historical minimum price. We update the `buy` price to the current `sell` price to maximize future potential profits.

---

## 演算法詳解

我們可以用一個簡單的狀態變化來理解這個過程。假設價格陣列為 `[7, 1, 5, 3, 6, 4]`：

* **初始狀態**：`buy = 7`, `profit = 0`
* **Step 1 (sell = 1)**：`1 < 7`，發現更便宜的買點。更新 `buy = 1`。
* **Step 2 (sell = 5)**：`5 > 1`，有利潤 $5 - 1 = 4$。更新 `profit = max(0, 4) = 4`。
* **Step 3 (sell = 3)**：`3 > 1`，有利潤 $3 - 1 = 2$。不大於當前最大利潤 4，`profit` 維持 4。
* **Step 4 (sell = 6)**：`6 > 1`，有利潤 $6 - 1 = 5$。更新 `profit = max(4, 5) = 5`。
* **Step 5 (sell = 4)**：`4 > 1`，有利潤 $4 - 1 = 3$。不大於 5，`profit` 維持 5。
* **結束**：回傳最大利潤 5。

---

## 程式碼實作

以下為時間複雜度最優的單次遍歷 (One-pass) 實作：

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
            
        profit = 0
        buy = prices[0]
        
        for sell in prices:
            if sell > buy: 
                # 表示有賺，因此比較是否比目前的最高利潤還多
                profit = max(profit, sell - buy)
            else: 
                # 發現更低的價格，更新未來的購買成本基準
                buy = sell
                
        return profit
```

## 正確性說明
* **確保不重複與不遺漏**：
    * 演算法掃描了陣列中每一個可能的「賣出日」。
    * 對於任何日期 $i$，最大的利潤一定是 $prices[i] - \min(prices[0...i-1])$。 由於我們在遍歷時持續更新 `buy` 為 $0$ 到 $i$ 的最小值，因此能確保涵蓋所有獲利可能。
* **處理重複與邊界**：
    * 若價格持續下跌（如 `[5, 4, 3, 2, 1]`），`sell` 永遠不會大於 `buy`，利潤保持為 0，符合題目要求。
    * 若出現重複的最低價，`else` 邏輯會更新 `buy`，這不會影響當前的利潤計算，但為後續更高的賣出價做好了準備。

## 複雜度分析
* **Time Complexity**: $O(n)$
    * 我們僅對陣列進行了一次線性掃描，其中 $n$ 為 `prices` 的長度。
* **Space Complexity**: $O(1)$
    * 只使用了兩個常數級變數 `profit` 與 `buy`，與輸入規模無關。

## 常見誤區
* **暴力解法 (Brute Force)**：初學者容易使用雙層迴圈計算每一對 $(i, j)$ 的差值，時間複雜度為 $O(n^2)$，這在資料量大時會導致超時 (TLE)。
* **忽視交易順序**：誤以為只要用 $\max(prices) - \min(prices)$ 即可。 必須記住「先買後賣」的限制，最小值必須出現在最大值之前。
* **未考慮負利潤**：題目要求若無法獲利則回傳 0，因此 `profit` 的初始值應設為 0，且只在差值為正時更新。

## 小結
* 本題是**動態規劃 (Dynamic Programming)** 與**滑動視窗 (Sliding Window)** 思想的簡化版。
* 它教會我們如何在一次遍歷中透過維護「局部最優解（當前最低價）」來推導出「全域最優解（最大利潤）」。