# [Day31 二分搜尋法Binary Search](https://ithelp.ithome.com.tw/articles/10280844)
二分搜尋法(Binary Search )，在執行前有一項必須條件，資料列需要是已排序好的狀態，因此若資料龐大且未排序，需要先搭配使用前面幾天介紹的排序法，再來執行二分搜尋法。
原理是在已排序好的資料列中找出中間值，再將搜尋的目標值與中間值作比較，如果目標值小於中間值，則往左邊資料列尋找，如果目標值大於中間值，則往右邊資料列尋找，藉此判斷目標值在資料列左邊或是右邊，每一回都透過選擇中間值來比較來縮小一半搜尋範圍，再重複前面步驟，直到搜尋到或確定不存在為止。

![20121027rqGyJGUz83](https://hackmd.io/_uploads/BJm-I2Wf0.jpg)

#### JavaScript
```javascript=
let data=[10,15,25,40,45,55,60,80,90];
let target=55;
function binarySearch(arr, target){
    let start = 0;
    let end = arr.length - 1;
    let mid;
    while(start <= end){
        mid = Math.floor( (start + end ) / 2)
        if(target < arr[mid]){
            end = mid - 1;
        }else if(target > arr[mid]){
            start = mid + 1
        }else{
            return "有搜尋結果: 在第" + (mid+1) + "項";
        }
    }
    return "無搜尋結果";
}
console.log(binarySearch(data,target));//"有搜尋結果: 在第6項"
```

#### Python
```python=
#Binary Search
data = [10,15,25,40,45,55,60,80,90]
target = 55
def binarySearch(arr, target):
    start = 0
    end = len(arr)-1
    while start <= end:
        mid = int((start + end) / 2)
        if target == arr[mid]:
            return "有搜尋結果: 在第" + str(mid+1) + "項"
        elif target > arr[mid]:
            start = mid + 1
        else:
            end = mid - 1
    return "無搜尋結果"

print(binarySearch(data,target))#有搜尋結果: 在第6項
```

# [Day32 內插搜尋法Interpolation Search](https://ithelp.ithome.com.tw/articles/10281248)
內插搜尋法(Interpolation Search  )，又稱插補搜尋法，是二分搜尋法的改良版，二分搜尋法是先找出中間值，而內插搜尋法是透過斜率公式來估出資料可能存在的位置，搜尋前也必須先將資料列排序好。

![20121027gqrEoCTvTW](https://hackmd.io/_uploads/SJydzsHGC.jpg)

搜尋**40**
![20121027CvgzhXuanp](https://hackmd.io/_uploads/SyPOfiBzR.jpg)

#### JavaScript
```javascript=
let data = [10,20,30,40,50,60,70,80,90];
let target = 40;
function interpolationSearch(arr, target) {
  let start = 0;
  let end = arr.length - 1;
  let mid;
  while (end >= start) {
    mid = Math.floor((target - arr[start]) * (end - start) / (arr[end] - arr[start]) + start);
    if (arr[mid] === target) {
      return "有搜尋結果: 在第" + (mid + 1) + "項";
    } else {
      if (target > arr[mid]) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
  }
  return "無搜尋結果";
}
console.log(interpolationSearch(data, target)); //"有搜尋結果: 在第4項"
```

#### Python
```python=
#Interpolation Search
data = [10,20,30,40,50,60,70,80,90]
target = 40
def interpolationSearch(arr, target):
    start = 0
    end = len(arr)-1
    while start <= end:
        mid = (target - arr[start]) * (end - start) // (arr[end] - arr[start]) + start
        if target == arr[mid]:
            return "有搜尋結果: 在第" + str(mid+1) + "項"
        else:
            if target < arr[mid]:
                end = mid - 1
            else:
                start = mid + 1
    return "無搜尋結果"

print(interpolationSearch(data,target))#有搜尋結果: 在第4項
```

# [Day33 深度優先搜尋DFS與廣度優先搜尋BFS](https://ithelp.ithome.com.tw/articles/10281404)
深度優先搜尋(Depth-First Search,DFS)與廣度優先搜尋(Breadth-First Search, BFS)，是可以用來走訪或搜尋樹節點與圖頂點的演算法，先前介紹的二元樹走訪就是使用上述方法走訪各節點，這邊以圖結構來介紹.

![20121027Sag2cgA5z3](https://hackmd.io/_uploads/Sk0blePfA.jpg)

## 深度優先搜尋DFS
先選定一個頂點開始走訪，接著從此頂點相鄰未被走過的頂點中，擇一走訪標示為記錄點，以此類推，不斷從新記錄點的相鄰未被走過頂點中尋找。
若新紀錄點的相鄰頂點都被走過，則退回前一個紀錄點，繼續從未被走過頂點中尋找。
深度優先可以利用堆疊(Stack)的方式來處理。
![201210276jKV7VdkNs](https://hackmd.io/_uploads/H1ksc-vMC.jpg)

## 廣度優先搜尋BFS
先選定一個頂點開始走訪，逐一走過此頂點相鄰未被走過的頂點，若相鄰頂點都被走過，再從走訪過的頂點中擇一為新記錄點，逐一走過新記錄點相鄰未被走過的頂點，以此類推。
廣度優先可以利用佇列(Queue)的方式來處理。
![20121027TnPd6lTzWS](https://hackmd.io/_uploads/HkZei-PzC.jpg)

#### JavaScript
```javascript=
class Graph {
    constructor() {
        this.adjacencyList = {}
    }
    //新增頂點
    addVertex(vertex) {
        if (!this.adjacencyList[vertex]) {
            this.adjacencyList[vertex] = []
        } else {
            return '頂點已存在';
        }
    }
    //新增邊
    addEdge(vertex1, vertex2) {
        if (this.adjacencyList[vertex1]) {
            if (this.adjacencyList[vertex2]){
                this.adjacencyList[vertex1].push(vertex2)
                this.adjacencyList[vertex2].push(vertex1)
            }else {
                return '第二項頂點不存在';
            }
        } else {
            return '第一項頂點不存在';
        }
    }
    //刪除頂點
    removeVertex(vertex) {
        if (this.adjacencyList[vertex]) {
            this.adjacencyList[vertex].forEach(function(item) {
                this.removeEdge(vertex, item)
                delete this.adjacencyList[vertex]
            });
        } else {
            return '此頂點已不存在';
        }
    }
    //刪除邊
    removeEdge(vertex1, vertex2) {
        if (this.adjacencyList[vertex1]) {
            if (this.adjacencyList[vertex2]){
                this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
                    (vertex) => vertex !== vertex2
                )
                this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
                    (vertex) => vertex !== vertex1
                )
            }else {
                return '第二項頂點已不存在';
            }
        } else {
            return '第一項頂點已不存在';
        }
    }
    printGraph(){
        console.log(this.adjacencyList)
    }
    //廣度優先
    bfs(start) {
        const queue = [start];
        const result = [];
        const visited = {};
        visited[start] = true;
        let currentVertex;
        while (queue.length) {
            currentVertex = queue.shift();
            result.push(currentVertex);
            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            });
        }
        return result;
    }
    //深度優先
    dfs(start) {
        const result = [];
        const stack = [start];
        const visited = {};
        visited[start] = true;
        let currentVertex;
        while (stack.length) {
            currentVertex = stack.pop();
            result.push(currentVertex);
            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    stack.push(neighbor);
                }
            });
        }
        return result;
    }
}

let graph = new Graph();
graph.addVertex('A');
graph.addVertex('B');
graph.addVertex('C');
graph.addVertex('D');
graph.addVertex('E');
graph.addVertex('F');
graph.addEdge('A', 'B');
graph.addEdge('A', 'D');
graph.addEdge('A', 'E');
graph.addEdge('B', 'C');
graph.addEdge('D', 'E');
graph.addEdge('E', 'F');
graph.addEdge('E', 'C');
graph.printGraph();
//{
// A: ["B", "D", "E"],
// B: ["A", "C"],
// C: ["B", "E"],
// D: ["A", "E"],
// E: ["A", "D", "F", "C"],
// F: ["E"]
//}
console.log(graph.bfs('A')) //["A", "B", "D", "E", "C", "F"]
console.log(graph.dfs('A')) //["A", "E", "C", "F", "D", "B"]
console.log(graph.dfs('B')) //["B", "C", "E", "F", "D", "A"]
console.log(graph.dfs('C')) //["C", "E", "F", "D", "A", "B"]
console.log(graph.dfs('D')) //["D", "E", "C", "B", "F", "A"]
console.log(graph.dfs('E')) //["E", "C", "B", "F", "D", "A"]
console.log(graph.dfs('F')) //["F", "E", "C", "B", "D", "A"]
```

#### Python
```python=
#Graph
graph = {
  'A': ["B", "D", "E"],
  'B': ["A", "C"],
  'C': ["B", "E"],
  'D': ["A", "E"],
  'E': ["A", "D", "F", "C"],
  'F': ["E"]
}

def bfs(graph,start):
    queue = []
    queue.append(start)
    result = []
    visited = set()
    visited.add(start)
    while(len(queue)>0):
        currentVertex = queue.pop(0)
        result.append(currentVertex)
        for neighbor in graph[currentVertex]:
            if neighbor not in visited:
                queue.append(neighbor)
                visited.add(neighbor)
    return result

def dfs(graph,start):
    stack = []
    result = []
    stack.append(start)
    visited = set()
    visited.add(start)
    while(len(stack)>0):
        currentVertex = stack.pop()
        result.append(currentVertex)
        for neighbor in graph[currentVertex]:
            if neighbor not in visited:
                stack.append(neighbor)
                visited.add(neighbor)
    return result

print(bfs(graph,'A')) #['A', 'B', 'D', 'E', 'C', 'F']
print(dfs(graph,'A')) #['A', 'E', 'C', 'F', 'D', 'B']
```

# [Day34 費波那契數列Fibonacci Sequence](https://ithelp.ithome.com.tw/articles/10281811)
之前在遞迴的篇章有介紹過費波那契數列，是使用遞迴的方式實作，但是從下面遞迴的樹狀圖來看，會發現有很多重複的節點，遞迴的深度越深，重複計算的節點也就越多，甚至會出現RecursionError的情況，而這樣的時間複雜度為$O(2^n)$。

![20121027PQhJA2oGXY](https://hackmd.io/_uploads/SkTncB_fR.jpg)

為了避免重複計算，==因此可以將會重複計算的部分暫存起來，避免重覆計算==來增加處理效能。

```python=
def fibonacci(n):
    if n == 0 or n == 1:
        return n
    seq = [0 for _ in range(n + 1)]
    seq[0] = 0
    seq[1] = 1
    for i in range(2, n + 1):
        seq[i] = seq[i-1] + seq[i-2]
    return seq[n]

fibonacci(8)#21
```

* 建立一個seq列表
* seq[i]為F(i)的值，可以說seq[i] = seq[i-1] + seq[i-2]
* 把seq[0]為0、seq[1]為1
* 用迴圈計算i=2到n+1時，得出seq[i]的結果，儲存於seq列表中被使用
* 最後算出來的seq[n]，就是F(n)的值

透過上面的方式，可以發現真正被計算到的只剩下橘色部分，藍色框為已經被儲存在seq列表中，不需要重複計算，時間複雜度變為$O(n)$
![201210270aqGI7Z1HW](https://hackmd.io/_uploads/HJuHiHOzC.jpg)

根據上述方式，空間複雜度為$O(n)$，但還可以有更進階的方式來==節省空間==。

```python
def fibonacci(n):
    if n == 0 or n == 1:
        return n
    a = 0
    b = 1
    for i in range(2, n+1):
        c = a + b
        a = b
        b = c
    return b

fibonacci(8)#21
```

* 開始的前兩項a與b，為0與1
* 後一項c是前兩項的加總
* 只需要對a和b不斷重新賦值
空間複雜度就能從原本的$O(n)$變成$O(1)$。

# [Day35 常見的演算法策略Algorithmic Patterns](https://ithelp.ithome.com.tw/articles/10281926)

## Divide and conquer (分治法)
最常被使用的策略方式，原理是將一個難以直接解決的大問題，依據相同的概念分割成多個子問題，再各個擊破，分而治之，不斷地將子問題縮小，最後再將各子問題的解答合併起來。
* 合併排序
* 快速排序
* 基數排序
* 二分搜尋

## Dynamic Programming (動態規劃法)
動態規劃法類似上述的分治法，依樣是將大問題分割成多個子問題來解決，只是有些子問題可能是重複的，所以會發生重複計算的問題，動態規劃與分治法最大不同在於會將重複計算的子問題將其記憶化儲存，來解決重複計算的問題，用空間換取時間的概念來加速執行效能。
* 改良版費波那契數列就是運用這樣的策略

## Greed Algorithm (貪婪法)
又稱貪心法，原理是在每一次解決步驟時都以貪心為原則，採取當下最有利的選擇，當步驟都結束後進而希望是最有利的解答。
假設今天你今天用一張100元鈔票買了18元飲料，你希望全部找的零錢硬幣數量要是最少，該如何找錢?
一開始先選擇50元 x1
接著10元 x3
再來5元 x0
最後1元 x2
總共6枚硬幣，最佳解答。

## Backtracking (回溯法)
原理是先列出此階段的所有分支可能性，接著排除掉不可能為解答的分支，再來往其中一個分支執行，若此階段已經確定所有分支都不為解答時，則退回上一階段，往其他分支執行，來避免多餘無效的步驟。
* 深度優先搜尋

## Branch and bound (分支界定法)
有點類似地毯式搜索，列出此階段的所有分支可能性，開始執行此階段所有分支，若此階段都不為解答時，其中一個分支成為新階段，重複上述步驟執行。
* 廣度優先搜尋
