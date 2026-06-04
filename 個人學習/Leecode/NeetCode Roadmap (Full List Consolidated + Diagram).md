---
title: NeetCode Roadmap (Full List Consolidated + Diagram)
date: 2026-06-01
tags:
  - leetcode
  - roadmap
status: active
description: "[source](https://neetcode.io/roadmap)"
---

說明 (繁體中文):
本版修正：移除 Stack → (Binary Search / Sliding Window / Linked List) 的箭頭。Stack 保留為獨立補充技巧節點，不直接推進主線。  

---

## Roadmap Diagram (Mermaid)

```mermaid
flowchart TB
    A[Arrays & Hashing] --> B[Two Pointers]
    A --> C[Stack]

    B --> D[Binary Search]
    B --> E[Sliding Window]
    B --> F[Linked List]

    D --> G[Trees]
    F --> G

    G --> H[Tries]
    G --> I[Backtracking]
    G --> J[Heap / Priority Queue]

    I --> K[Graphs]
    I --> L[1-D DP]

    J --> M[Intervals]
    J --> N[Greedy]
    J --> O[Advanced Graphs]

    K --> O
    K --> P[2-D DP]
    K --> R[Math & Geometry]

    L --> P
    L --> Q[Bit Manipulation]

    
    Q --> R
```

## Arrays & Hashing
| Problem                      | LeetCode                                                   | Difficulty | Key Points                     |              筆記               |  Progress  |
| ---------------------------- |:---------------------------------------------------------- |:---------- | ------------------------------ |:-------------------------------:|:----------:|
| Contains Duplicate           | https://leetcode.com/problems/contains-duplicate           | Easy       | HashSet duplicate check        |                                | 2025.12.18 |
| Valid Anagram                | https://leetcode.com/problems/valid-anagram                | Easy       | Frequency count                |                                | 2025.12.17 |
| Two Sum                      | https://leetcode.com/problems/two-sum                      | Easy       | Hash map complement            |                                | 2025.12.18 |
| Group Anagrams               | https://leetcode.com/problems/group-anagrams               | Medium     | Sorted key / count signature   |                                | 2025.12.24 |
| Top K Frequent Elements      | https://leetcode.com/problems/top-k-frequent-elements      | Medium     | Bucket / heap                  |                                | 2026.01.15 |
| Encode and Decode Strings    | https://leetcode.com/problems/encode-and-decode-strings    | Medium     | Custom delimiter length-prefix |                                |            |
| Product of Array Except Self | https://leetcode.com/problems/product-of-array-except-self | Medium     | Prefix & suffix pass           |                                |            |
| Valid Sudoku                 | https://leetcode.com/problems/valid-sudoku                 | Medium     | Set constraints (row/col/box)  |                                |            |
| Longest Consecutive Sequence | https://leetcode.com/problems/longest-consecutive-sequence | Medium     | Hash set starts                |                                |            |

## Two Pointers
| Problem                            | LeetCode                                                       | Difficulty | Key Points                  |              筆記               |  Progress  |
| ---------------------------------- | -------------------------------------------------------------- | ---------- | --------------------------- |:-------------------------------:|:----------:|
| Valid Palindrome                   | https://leetcode.com/problems/valid-palindrome                 | Easy       | Filter + inward scan        |                                | 2026.01.16 |
| Two Sum II - Input Array Is Sorted | https://leetcode.com/problems/two-sum-ii-input-array-is-sorted | Medium     | Opposite pointers           |                                | 2026.01.16 |
| 3Sum                               | https://leetcode.com/problems/3sum                             | Medium     | Sort + skip duplicates      |                                | 2026.01.19 |
| Container With Most Water          | https://leetcode.com/problems/container-with-most-water        | Medium     | Move smaller pointer        |                                | 2026.01.20 |
| Trapping Rain Water                | https://leetcode.com/problems/trapping-rain-water              | Hard       | Two pointer / prefix maxima |                                | 2026.01.22 |

## Sliding Window
| Problem                                        | LeetCode                                                                     | Difficulty | Key Points             |              筆記               |  Progress  |
| ---------------------------------------------- | ---------------------------------------------------------------------------- | ---------- | ---------------------- |:-------------------------------:|:----------:|
| Best Time to Buy and Sell Stock                | https://leetcode.com/problems/best-time-to-buy-and-sell-stock                | Easy       | Track min so far       |                                | 2026.02.23 |
| Longest Substring Without Repeating Characters | https://leetcode.com/problems/longest-substring-without-repeating-characters | Medium     | Window + index map     |                                | 2025.10.14 |
| Longest Repeating Character Replacement        | https://leetcode.com/problems/longest-repeating-character-replacement        | Medium     | Window + max count     |                                | 2025.10.14 |
| Minimum Window Substring                       | https://leetcode.com/problems/minimum-window-substring                       | Hard       | Expand/contract counts |                                |            |
| Sliding Window Maximum                         | https://leetcode.com/problems/sliding-window-maximum                         | Hard       | Monotonic deque        |                                |            |

## Stack
| Problem                          | LeetCode                                                       | Difficulty | Key Points                     |              筆記               |  Progress  |
| -------------------------------- |:-------------------------------------------------------------- | ---------- | ------------------------------ |:-------------------------------:|:----------:|
| Valid Parentheses                | https://leetcode.com/problems/valid-parentheses                | Easy       | Stack matching                 |                                | 2025.10.15 |
| Min Stack                        | https://leetcode.com/problems/min-stack                        | Medium     | Track min per node             |                                | 2025.10.15 |
| Evaluate Reverse Polish Notation | https://leetcode.com/problems/evaluate-reverse-polish-notation | Medium     | Stack ops                      |                                | 2025.10.15 |
| Generate Parentheses             | https://leetcode.com/problems/generate-parentheses             | Medium     | Backtrack counts               |                                | 2025.10.16 |
| Daily Temperatures               | https://leetcode.com/problems/daily-temperatures               | Medium     | Monotonic stack                |                                | 2025.10.16 |
| Car Fleet                        | https://leetcode.com/problems/car-fleet                        | Medium     | Sort by position / stack times |                                | 2025.10.17 |
| Largest Rectangle in Histogram   | https://leetcode.com/problems/largest-rectangle-in-histogram   | Hard       | Monotonic stack sentinel       |                                |            |

## Binary Search
| Problem                              | LeetCode                                                           | Difficulty | Key Points            |              筆記               |  Progress  |
| ------------------------------------ | ------------------------------------------------------------------ | ---------- | --------------------- |:-------------------------------:|:----------:|
| Binary Search                        | https://leetcode.com/problems/binary-search                        | Easy       | Classic template      |                                | 2025.10.17 |
| Search a 2D Matrix                   | https://leetcode.com/problems/search-a-2d-matrix                   | Medium     | Flatten / 2-phase     |                                | 2025.10.21 |
| Koko Eating Bananas                  | https://leetcode.com/problems/koko-eating-bananas                  | Medium     | Search speed          |                                | 2025.10.21 |
| Find Minimum in Rotated Sorted Array | https://leetcode.com/problems/find-minimum-in-rotated-sorted-array | Medium     | Compare mid vs right  |                                | 2025.10.21 |
| Search in Rotated Sorted Array       | https://leetcode.com/problems/search-in-rotated-sorted-array       | Medium     | Determine sorted half |                                | 2025.10.22 |
| Time Based Key-Value Store           | https://leetcode.com/problems/time-based-key-value-store           | Medium     | Store list + bs       |                                | 2025.10.23 |
| Median of Two Sorted Arrays          | https://leetcode.com/problems/median-of-two-sorted-arrays          | Hard       | Partition halves      |                                |            |

## Linked List
| Problem                          | LeetCode                                                       | Difficulty | Key Points                      |               筆記                |  Progress  |
| -------------------------------- | -------------------------------------------------------------- | ---------- | ------------------------------- |:---------------------------------:|:----------:|
| Reverse Linked List              | https://leetcode.com/problems/reverse-linked-list              | Easy       | Iterative pointer flip          |                                  | 2025.10.27 |
| Merge Two Sorted Lists           | https://leetcode.com/problems/merge-two-sorted-lists           | Easy       | Dummy head merge                |                                  | 2025.10.28 |
| Reorder List                     | https://leetcode.com/problems/reorder-list                     | Medium     | Split + reverse + weave         |                                  | 2025.10.30 |
| Remove Nth Node From End of List | https://leetcode.com/problems/remove-nth-node-from-end-of-list | Medium     | Two pointers gap                |                                  | 2025.10.31 |
| Copy List with Random Pointer    | https://leetcode.com/problems/copy-list-with-random-pointer    | Medium     | Interweave clone nodes          |                                  |            |
| Add Two Numbers                  | https://leetcode.com/problems/add-two-numbers                  | Medium     | Carry addition                  |                                  | 2026.02.09 |
| Linked List Cycle                | https://leetcode.com/problems/linked-list-cycle                | Easy       | Floyd detect                    |                                  | 2026.03.04 |
| Find the Duplicate Number        | https://leetcode.com/problems/find-the-duplicate-number        | Medium     | Cycle detection / binary search |                                  | 2026.03.10 |
| LRU Cache                        | https://leetcode.com/problems/lru-cache                        | Medium     | Hash map + DLL                  |                                  |2026.03.12            |
| Merge k Sorted Lists             | https://leetcode.com/problems/merge-k-sorted-lists             | Hard       | Heap or divide                  |                                  |            |
| Reverse Nodes in k-Group         | https://leetcode.com/problems/reverse-nodes-in-k-group         | Hard       | Segment reversal                |                                  |            |

## Trees
| Category | Problem                                                   | LeetCode                                                                                | Difficulty |            Key Points             |              筆記               |  Progress  |
| -------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------- | ---------- |:---------------------------------:|:-------------------------------:|:----------:|
| Trees    | Invert Binary Tree                                        | https://leetcode.com/problems/invert-binary-tree                                        | Easy       |           Swap children           |                                | 2026.03.14 |
| Trees    | Maximum Depth of Binary Tree                              | https://leetcode.com/problems/maximum-depth-of-binary-tree                              | Easy       |             DFS depth             |                                | 2026.03.14 |
| Trees    | Diameter of Binary Tree                                   | https://leetcode.com/problems/diameter-of-binary-tree                                   | Easy       |      Track max path at node       |                                | 2026.03.18 |
| Trees    | Balanced Binary Tree                                      | https://leetcode.com/problems/balanced-binary-tree                                      | Easy       |  Height = -1 signals unbalanced   |                                | 2026.03.18 |
| Trees    | Same Tree                                                 | https://leetcode.com/problems/same-tree                                                 | Easy       |            DFS compare            |                                | 2026.03.19 |
| Trees    | Subtree of Another Tree                                   | https://leetcode.com/problems/subtree-of-another-tree                                   | Easy       |    Same tree check at matches     |                                |            |
| Trees    | Lowest Common Ancestor of a BST                           | https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree            | Medium     |         BST path compare          |                                |            |
| Trees    | Binary Tree Level Order Traversal                         | https://leetcode.com/problems/binary-tree-level-order-traversal                         | Medium     |             BFS queue             |                                |            |
| Trees    | Binary Tree Right Side View                               | https://leetcode.com/problems/binary-tree-right-side-view                               | Medium     | BFS last or DFS depth first right |                                |            |
| Trees    | Count Good Nodes in Binary Tree                           | https://leetcode.com/problems/count-good-nodes-in-binary-tree                           | Medium     |         Carry max so far          |                                |            |
| Trees    | Validate Binary Search Tree                               | https://leetcode.com/problems/validate-binary-search-tree                               | Medium     |          Min/max bounds           |                                |            |
| Trees    | Kth Smallest Element in a BST                             | https://leetcode.com/problems/kth-smallest-element-in-a-bst                             | Medium     |           Inorder count           |                                |            |
| Trees    | Construct Binary Tree from Preorder and Inorder Traversal | https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal | Medium     |        Recursion splitting        |                                |            |
| Trees    | Binary Tree Maximum Path Sum                              | https://leetcode.com/problems/binary-tree-maximum-path-sum                              | Hard       |      Max gain from children       |                                |            |
| Trees    | Serialize and Deserialize Binary Tree                     | https://leetcode.com/problems/serialize-and-deserialize-binary-tree                     | Hard       |         BFS or DFS format         |                                |            |

## Tries
| Category | Problem                                    | LeetCode                                                                 | Difficulty | Key Points                   | 筆記 | Progress |
| -------- | ------------------------------------------ | ------------------------------------------------------------------------ | ---------- | ---------------------------- | :---: | -------- |
| Tries    | Implement Trie (Prefix Tree)               | https://leetcode.com/problems/implement-trie-prefix-tree                 | Medium     | Node dict children           |                                |          |
| Tries    | Design Add and Search Words Data Structure | https://leetcode.com/problems/design-add-and-search-words-data-structure | Medium     | '.' wildcard DFS             |  |          |
| Tries    | Word Search II                             | https://leetcode.com/problems/word-search-ii                             | Hard       | Board backtrack + trie prune |  |          |

## Heap / Priority Queue
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Heap / Priority Queue | Kth Largest Element in a Stream | https://leetcode.com/problems/kth-largest-element-in-a-stream | Easy | Min-heap size k |  |  |
| Heap / Priority Queue | Last Stone Weight | https://leetcode.com/problems/last-stone-weight | Easy | Max-heap combine |  |  |
| Heap / Priority Queue | K Closest Points to Origin | https://leetcode.com/problems/k-closest-points-to-origin | Medium | Heap / quickselect |  |  |
| Heap / Priority Queue | Kth Largest Element in an Array | https://leetcode.com/problems/kth-largest-element-in-an-array | Medium | Quickselect or heap |  |  |
| Heap / Priority Queue | Task Scheduler | https://leetcode.com/problems/task-scheduler | Medium | Greedy counts + idle calc |  |  |
| Heap / Priority Queue | Design Twitter | https://leetcode.com/problems/design-twitter | Medium | Min-heap merge streams |  |  |
| Heap / Priority Queue | Find Median from Data Stream | https://leetcode.com/problems/find-median-from-data-stream | Hard | Two heaps balance |  |  |

## Backtracking
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Backtracking | Subsets | https://leetcode.com/problems/subsets | Medium | Decision include/exclude |  |  |
| Backtracking | Combination Sum | https://leetcode.com/problems/combination-sum | Medium | Reuse candidates |  |  |
| Backtracking | Permutations | https://leetcode.com/problems/permutations | Medium | Swap / used array |  |  |
| Backtracking | Subsets II | https://leetcode.com/problems/subsets-ii | Medium | Skip duplicates |  |  |
| Backtracking | Combination Sum II | https://leetcode.com/problems/combination-sum-ii | Medium | Single use candidates |                                |  |
| Backtracking | Word Search | https://leetcode.com/problems/word-search | Medium | DFS board marking |  |  |
| Backtracking | Palindrome Partitioning | https://leetcode.com/problems/palindrome-partitioning | Medium | Expand check prefix |  |  |
| Backtracking | Letter Combinations of a Phone Number | https://leetcode.com/problems/letter-combinations-of-a-phone-number | Medium | Digit mapping recursion |  |  |
| Backtracking | N-Queens | https://leetcode.com/problems/n-queens | Hard | Columns/diag sets |  |  |
| Backtracking | Sudoku Solver | https://leetcode.com/problems/sudoku-solver | Hard | Constraint sets |  |  |

## Graphs
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Graphs | Number of Islands | https://leetcode.com/problems/number-of-islands | Medium | DFS/BFS flood |                                |  |
| Graphs | Clone Graph | https://leetcode.com/problems/clone-graph | Medium | Map old->new |                                |  |
| Graphs | Max Area of Island | https://leetcode.com/problems/max-area-of-island | Medium | DFS counting |  |  |
| Graphs | Pacific Atlantic Water Flow | https://leetcode.com/problems/pacific-atlantic-water-flow | Medium | Reverse flow BFS/DFS |                                |  |
| Graphs | Surrounded Regions | https://leetcode.com/problems/surrounded-regions | Medium | Border escape mark |  |  |
| Graphs | Rotting Oranges | https://leetcode.com/problems/rotting-oranges | Medium | Multi-source BFS |  |  |
| Graphs | Walls and Gates | https://leetcode.com/problems/walls-and-gates | Medium | Multi-source BFS distances |  |  |
| Graphs | Course Schedule | https://leetcode.com/problems/course-schedule | Medium | Detect cycle (DFS / indegree) |                                |  |
| Graphs | Course Schedule II | https://leetcode.com/problems/course-schedule-ii | Medium | Topological order |  |  |
| Graphs | Graph Valid Tree | https://leetcode.com/problems/graph-valid-tree | Medium | n-1 edges + no cycle |  |  |
| Graphs | Number of Connected Components in an Undirected Graph | https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph | Medium | Union find / DFS |  |  |

## Advanced Graphs
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Advanced Graphs | Reconstruct Itinerary | https://leetcode.com/problems/reconstruct-itinerary | Hard | Hierholzer Eulerian path |  |  |
| Advanced Graphs | Min Cost to Connect All Points | https://leetcode.com/problems/min-cost-to-connect-all-points | Medium | Prim MST |  |  |
| Advanced Graphs | Network Delay Time | https://leetcode.com/problems/network-delay-time | Medium | Dijkstra |  |  |
| Advanced Graphs | Swim in Rising Water | https://leetcode.com/problems/swim-in-rising-water | Hard | Min-heap BFS |  |  |
| Advanced Graphs | Alien Dictionary | https://leetcode.com/problems/alien-dictionary | Hard | Topo order from edges |  |  |
| Advanced Graphs | Cheapest Flights Within K Stops | https://leetcode.com/problems/cheapest-flights-within-k-stops | Medium | Bellman-Ford layering |  |  |
| Advanced Graphs | Redundant Connection | https://leetcode.com/problems/redundant-connection | Medium | Union find detect cycle |  |  |
| Advanced Graphs | Course Schedule III | https://leetcode.com/problems/course-schedule-iii | Hard | Sort by end + max-heap durations |  |  |

## 1-D Dynamic Programming
| Category | Problem                        | LeetCode                                                     | Difficulty | Key Points              |              筆記               | Progress |
| -------- | ------------------------------ | ------------------------------------------------------------ | ---------- | ----------------------- |:-------------------------------:| -------- |
| 1-D DP   | Climbing Stairs                | https://leetcode.com/problems/climbing-stairs                | Easy       | Fib style               |                                |          |
| 1-D DP   | Min Cost Climbing Stairs       | https://leetcode.com/problems/min-cost-climbing-stairs       | Easy       | dp[i] cost to reach     |                                |          |
| 1-D DP   | House Robber                   | https://leetcode.com/problems/house-robber                   | Medium     | Rolling include/exclude |                                |          |
| 1-D DP   | House Robber II                | https://leetcode.com/problems/house-robber-ii                | Medium     | Circle split two runs   |                                |          |
| 1-D DP   | Longest Palindromic Substring  | https://leetcode.com/problems/longest-palindromic-substring  | Medium     | Expand centers          |                                |          |
| 1-D DP   | Palindromic Substrings         | https://leetcode.com/problems/palindromic-substrings         | Medium     | Expand centers count    |                                |          |
| 1-D DP   | Decode Ways                    | https://leetcode.com/problems/decode-ways                    | Medium     | dp[i] from last one/two |                                |          |
| 1-D DP   | Coin Change                    | https://leetcode.com/problems/coin-change                    | Medium     | Min coins dp            |                                |          |
| 1-D DP   | Maximum Product Subarray       | https://leetcode.com/problems/maximum-product-subarray       | Medium     | Track max & min         |                                |          |
| 1-D DP   | Word Break                     | https://leetcode.com/problems/word-break                     | Medium     | dp cut points           |                                |          |
| 1-D DP   | Longest Increasing Subsequence | https://leetcode.com/problems/longest-increasing-subsequence | Medium     | Patience piles          |                                |          |
| 1-D DP   | Partition Equal Subset Sum     | https://leetcode.com/problems/partition-equal-subset-sum     | Medium     | 1-D knap boolean        |                                |          |
| 1-D DP   | Maximum Subarray               | https://leetcode.com/problems/maximum-subarray               | Easy       | Kadane                  |                                |          |
| 1-D DP   | Jump Game                      | https://leetcode.com/problems/jump-game                      | Medium     | Greedy reachable        |                                |          |
| 1-D DP   | Jump Game II                   | https://leetcode.com/problems/jump-game-ii                   | Medium     | Layer greedy jumps      |                                |          |

## 2-D Dynamic Programming
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| 2-D DP | Unique Paths | https://leetcode.com/problems/unique-paths | Medium | Combinatorial / dp grid |  |  |
| 2-D DP | Longest Common Subsequence | https://leetcode.com/problems/longest-common-subsequence | Medium | 2D dp match/skip |  |  |
| 2-D DP | Best Time to Buy and Sell Stock with Cooldown | https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown | Medium | State machine |  |  |
| 2-D DP | Coin Change II | https://leetcode.com/problems/coin-change-ii | Medium | Ways accumulation |  |  |
| 2-D DP | Target Sum | https://leetcode.com/problems/target-sum | Medium | Offset sum dp |  |  |
| 2-D DP | Interleaving String | https://leetcode.com/problems/interleaving-string | Medium | dp[i][j] prefix |                                |  |
| 2-D DP | Longest Increasing Path in a Matrix | https://leetcode.com/problems/longest-increasing-path-in-a-matrix | Hard | DFS memo |  |  |
| 2-D DP | Distinct Subsequences | https://leetcode.com/problems/distinct-subsequences | Hard | 2D dp transitions |  |  |
| 2-D DP | Edit Distance | https://leetcode.com/problems/edit-distance | Medium | Insert/delete/replace |  |  |
| 2-D DP | Burst Balloons | https://leetcode.com/problems/burst-balloons | Hard | Interval DP |  |  |
| 2-D DP | Regular Expression Matching | https://leetcode.com/problems/regular-expression-matching | Hard | dp with '*' '.' |  |  |
| 2-D DP | Maximal Rectangle | https://leetcode.com/problems/maximal-rectangle | Hard | Histogram per row |  |  |

## Greedy
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Greedy | Maximum Subarray | https://leetcode.com/problems/maximum-subarray | Easy | Kadane reuse |                                |  |
| Greedy | Jump Game | https://leetcode.com/problems/jump-game | Medium | Furthest reach |  |  |
| Greedy | Jump Game II | https://leetcode.com/problems/jump-game-ii | Medium | BFS layer greedy |  |  |
| Greedy | Gas Station | https://leetcode.com/problems/gas-station | Medium | Single pass reset start |  |  |
| Greedy | Hand of Straights | https://leetcode.com/problems/hand-of-straights | Medium | Ordered map counts |  |  |
| Greedy | Merge Triplets to Form Target Triplet | https://leetcode.com/problems/merge-triplets-to-form-target-triplet | Medium | Track dominated triples |  |  |
| Greedy | Partition Labels | https://leetcode.com/problems/partition-labels | Medium | Last occurrence boundaries |  |  |
| Greedy | Valid Parenthesis String | https://leetcode.com/problems/valid-parenthesis-string | Medium | Range of open count |  |  |

## Intervals
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Intervals | Insert Interval | https://leetcode.com/problems/insert-interval | Medium | Merge on insert |                                |  |
| Intervals | Merge Intervals | https://leetcode.com/problems/merge-intervals | Medium | Sort + merge accumulate |                                |  |
| Intervals | Non-overlapping Intervals | https://leetcode.com/problems/non-overlapping-intervals | Medium | Greedy end keep |                                |  |
| Intervals | Meeting Rooms | https://leetcode.com/problems/meeting-rooms | Easy | Sort + check overlap |                                |  |
| Intervals | Meeting Rooms II | https://leetcode.com/problems/meeting-rooms-ii | Medium | Min-heap end times |  |  |
| Intervals | Minimum Interval to Include Each Query | https://leetcode.com/problems/minimum-interval-to-include-each-query | Hard | Sort queries + heap |  |  |
| Intervals | Interval List Intersections | https://leetcode.com/problems/interval-list-intersections | Medium | Two pointers |  |  |

## Math & Geometry
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Math & Geometry | Rotate Image | https://leetcode.com/problems/rotate-image | Medium | Transpose + reverse |  |  |
| Math & Geometry | Spiral Matrix | https://leetcode.com/problems/spiral-matrix | Medium | Layer traversal |  |  |
| Math & Geometry | Set Matrix Zeroes | https://leetcode.com/problems/set-matrix-zeroes | Medium | First row/col markers |  |  |
| Math & Geometry | Happy Number | https://leetcode.com/problems/happy-number | Easy | Cycle detection sum squares |  |  |
| Math & Geometry | Plus One | https://leetcode.com/problems/plus-one | Easy | Carry propagate |  |  |
| Math & Geometry | Pow(x, n) | https://leetcode.com/problems/powx-n | Medium | Fast exponentiation |  |  |
| Math & Geometry | Multiply Strings | https://leetcode.com/problems/multiply-strings | Medium | Grade-school multiply |  |  |
| Math & Geometry | Detect Squares (Optional extension) | https://leetcode.com/problems/detect-squares | Medium | Count points by x |  |  |

## Bit Manipulation
| Category | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|----------|---------|----------|------------|------------| :---: |----------|
| Bit Manipulation | Single Number | https://leetcode.com/problems/single-number | Easy | XOR pairs |  |  |
| Bit Manipulation | Number of 1 Bits | https://leetcode.com/problems/number-of-1-bits | Easy | n &= n-1 loop |  |  |
| Bit Manipulation | Counting Bits | https://leetcode.com/problems/counting-bits | Easy | dp i&(i-1) +1 |  |  |
| Bit Manipulation | Reverse Bits | https://leetcode.com/problems/reverse-bits | Easy | Shift accumulate |  |  |
| Bit Manipulation | Missing Number | https://leetcode.com/problems/missing-number | Easy | XOR indices+values |  |  |
| Bit Manipulation | Sum of Two Integers | https://leetcode.com/problems/sum-of-two-integers | Medium | Bit add w/o + |  |  |
| Bit Manipulation | Bitwise AND of Numbers Range | https://leetcode.com/problems/bitwise-and-of-numbers-range | Medium | Common prefix shift |  |  |

---

## Extended / Additional (If Roadmap Expanded)
| Category (Extended) | Problem | LeetCode | Difficulty | Key Points | 筆記 | Progress |
|---------------------|---------|----------|------------|------------| :---: |----------|
| Monotonic / Stack | Next Greater Element I | https://leetcode.com/problems/next-greater-element-i | Easy | Stack map |  |  |
| Monotonic / Stack | Next Greater Element II | https://leetcode.com/problems/next-greater-element-ii | Medium | Circular stack |  |  |
| Arrays | 4Sum | https://leetcode.com/problems/4sum | Medium | k-sum reduction |  |  |
| Heap | Top K Frequent Words | https://leetcode.com/problems/top-k-frequent-words | Medium | Heap + lexical |  |  |
| Graphs | Dijkstra Variants (Shortest Path to Get All Keys) | https://leetcode.com/problems/shortest-path-to-get-all-keys | Hard | BFS state bitmask |  |  |
| DP | Word Break II | https://leetcode.com/problems/word-break-ii | Hard | DFS with memo |  |  |
| DP | Longest Palindromic Subsequence | https://leetcode.com/problems/longest-palindromic-subsequence | Medium | Reverse LCS or dp[i][j] |  |  |
| Math | Random Pick with Weight | https://leetcode.com/problems/random-pick-with-weight | Medium | Prefix + binary search |  |  |
| Greedy | Advantage Shuffle | https://leetcode.com/problems/advantage-shuffle | Medium | Sort & assign |  |  |

---

## Progress Tracking 提示
- 搜尋 `|  |` 批次替換為 `| ✅ |` 表示已完成。  
- 可轉 CSV 後用試算表做進度統計。  

## 後續可選強化
1. 加上 Tags 欄：`two-pointers`, `monotonic-stack`, `union-find`, `prefix-sum` …  
2. 加上 Time / Space Complexity 欄。  
3. 產生多語系版本 (英文/繁中)。  
4. 自動化腳本：用 `grep` / `awk` 統計完成率。

---

如需再補充、精簡或生成子清單（例如只顯示 Hard），告訴我即可。  