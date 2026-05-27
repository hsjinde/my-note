# [Day 11 遞迴Recursion](https://ithelp.ithome.com.tw/articles/10269528)
遞迴(Recursion)的概念是將一個==大的問題，分割成許多小問題==去解決。

## 實作階層
```python!
def factorial(n):
    if n == 1:
        return 1
    else:
        return (n * factorial(n - 1))

print(factorial(5))#120
```
執行過程為下圖
![20121027nwuXjcARyP](https://hackmd.io/_uploads/H1hqvBCCp.jpg)

### 遞回函式條件:
* 重複執行它自己
* 跳出執行的出口

## 費式數列(Fibonacci Sequence)
```python!
def fibonacci(n):
    if n<1:
        return 0
    elif n==1:
        return 1
    else:
        return fibonacci(n-1)+fibonacci(n-2)
print(fibonacci(5)) #5
```
![20121027gIMrN89S88](https://hackmd.io/_uploads/S10L_S0Ra.jpg)

# [Day 12 樹Tree](https://ithelp.ithome.com.tw/articles/10270326)
樹(Tree)屬於一種非線性結構，是一種上下階層關係，舉例: 組織架構圖、家族譜、賽程表等，類似一棵倒過來的樹，從一個樹根(root)開始向下發展許多節點(node)。

## 樹的特性
* 不能構成迴路，因此任兩個節點只能有唯一一條邊。
* 一棵樹若有 n 個節點，一定有 n-1 條邊。

![20121027KBI9Remgkt](https://hackmd.io/_uploads/B19MM8RCp.jpg)

## 樹結構名稱
![20121027Ayxh0NIhqa](https://hackmd.io/_uploads/rk57G8CCT.jpg)

1. Node(節點)：每個Tree所連接到的點，都可被稱作這棵樹的Node(節點)。
2. Root(根節點或樹根)：每個Tree最初(或最頂層)的節點，每個Tree都只有一個Root，如: A節點。
3. Parent(父節點)：若該節點有下一層連結節點，則該節點為它的父節點，如: B是E的父節點。
4. Children (子節點)：若該節點有上一層連結節點，則該節點為它的子節點，如: D、E是B的子節點。
5. Siblings (兄弟節點):有共同的父節點之節點，它們稱為兄弟節點，如: F、G為兄弟節點。
6. Leaf(葉節點)/ Terminal(終端節點)：沒有子節點的節點，如: H、I、E、F、G都是葉節點/終端節點。
7. Level(階層)：該節點所在的水平層級，如: B的階層為2。
8. Degree (分支度):該節點的子節點總數，如: B的分支度為2。
9. Depth(深度)/ Height(高度)：這棵Tree的總共階層，上圖Tree的深度/高度為4。

# [Day13 二元樹Binary Tree](https://ithelp.ithome.com.tw/articles/10270953)
二元樹(Binary Tree)是最廣泛被使用的樹狀資料結構，簡單來說即為每個節點==最多只能有兩個子節點==。

## 樹與二元樹不同之處
* 樹不能是空集合，二元樹可以是空集合。
* 樹的分支度是 d ≥ 0，二元樹的分支度是 0 ≤ d ≤ 2。
* 樹的左右沒有次序關係，二元樹的左右有次序關係。

### 二元樹的種類
* 完全二元樹(Complete Binary Tree)
* 一棵二元樹中，除了最後一層不是滿的，其餘層都是滿的。
![20121027aY9Df1to3c](https://hackmd.io/_uploads/S15XjckJR.jpg)

* 滿二元樹(Fully Binary Tree)
* 每一層節點數都是最大節點數量。
![20121027oDkVmp8hX1](https://hackmd.io/_uploads/BJ_EiqyJ0.jpg)

* 歪斜樹(Skewed Binary Tree)
* 一棵二元樹中，完全沒有左邊或右邊節點，如果集中左邊為「左歪斜樹」，集中右邊為「右歪斜樹」。
![20121027y5kvvQKd6k](https://hackmd.io/_uploads/SyyHoqk1A.jpg)

### 常見二元樹的實作方式
1. 陣列表示法
根節點放在陣列索引位置 0。
若父節點索引為 i：
* 父節點的左子節點，放在陣列索引位置 (2 * i) + 1。
* 父節點的右子節點，放在陣列索引位置 (2 * i) + 2。
* 若節點不為根節點，節點的父節點放在陣列索引位置 (i - 1) / 2（取商）。
![20121027qtbTbwxGY8](https://hackmd.io/_uploads/B1rih51JR.jpg)

2. 鏈結串列表示法
節點含有兩個指標，左右指標分別指向左邊的子節點與右邊的子節點。
![20121027MMrsu27ECV](https://hackmd.io/_uploads/BJ-a3qJ1A.jpg)

# [Day14 二元樹走訪Binary Tree Traversal](https://ithelp.ithome.com.tw/articles/10271647)
- 深度優先搜尋（Depth-first Search，DFS）
    - 從根節點出發，選擇某一子樹並以垂直方向由上到下處理，將其子節點訪問完後，再選擇另一子樹走訪。
- 廣度優先搜尋（Breath-first Search，BFS）
    - 從根節點出發，以水平方向由左到右走訪，將同階層的兄弟節點訪問完之後，再走訪下一階層的節點。

## 深度優先搜尋 DFS
![20121027KNAP2Q2EsN](https://hackmd.io/_uploads/HyeM0k-JC.jpg)
- 前序走訪(Pre-order Traversal): **NLR, 根節點 → 左子樹 → 右子樹**
- 中序走訪(In-order Traversal): **LNR, 左子樹 → 根節點 → 右子樹**
- 後序走訪(Post-order Traversal): **LRN, 左子樹 → 右子樹 → 根節點**

## 廣度優先搜尋 BFS
![20121027ttoGqdrCdi](https://hackmd.io/_uploads/ryrHyg-k0.jpg)
層序走訪(Level-order Traversal): 1234567

---

![20121027j1S82bAe4Z](https://hackmd.io/_uploads/S12VzgbJR.jpg)

1. 前序走訪：
順序是根節點、左子節點、右子節點。
[1, 2, 4, 8, 9, 5, 3, 6, 10, 11, 7]
2. 中序走訪：
順序是左子節點、根節點、右子節點。
[8, 4, 9, 2, 5, 1, 10, 6, 11, 3, 7]
3. 後序走訪歷：
順序是左子節點、右子節點、根節點。
[8, 9, 4, 5, 2, 10, 11, 6, 7, 3, 1]
4. 層序走訪：
順序是由根節點一層一層往下，由左往右。
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

# [Day15 二元搜尋樹Binary Search Tree, BST](https://ithelp.ithome.com.tw/articles/10272328)
二元搜尋樹(Binary Search Tree)，也稱有序/排序二元樹，是一種特殊二元樹結構，而節點資料的排序具備一些特性。
---
_左子樹任一節點的值一定小於根節點的值。
右子樹任一節點的值一定大於根節點的值。
--任一節點的左、右子樹也分別為二元搜尋樹。
每一個節點不能出現重複的資料。
---
如下面這棵樹，就是一棵合法的BST
![201210278DRvVFrlPp](https://hackmd.io/_uploads/S1zITebJC.jpg)
下面這棵樹就不是一棵合法的BST，雖然節點11大於8是合法，大於6也是合法的。但是不能大於10這個節點。畢竟11這個節點還是屬於10節點的左子樹。因此不是一棵合法的二元搜尋樹。
![20121027EVkypo1bMq](https://hackmd.io/_uploads/Hy-8Cl-yR.jpg)

## 節點的新增、搜尋、刪除

### 新增節點
- 若目標值小於節點的值，則前往左子樹；若目標值大於節點的值，則前往右子樹；
![201210270lxK9JhbHC](https://hackmd.io/_uploads/rJ_SyZb10.jpg)

### 搜尋
- 若目標值小於節點的值，則繼續在左子樹中搜尋；若目標值大於節點的值，則繼續在右子樹中搜尋；
![20121027wJbss1oGne](https://hackmd.io/_uploads/H11sJWbyA.jpg)

### 刪除
- 葉子節點（無子樹），直接刪除。
![20121027r4HlHL1H5o](https://hackmd.io/_uploads/BkglgbbJR.jpg)

- 節點有單邊子樹，用子樹代替該節點。
![201210278r141V68fb](https://hackmd.io/_uploads/S1Clg-by0.jpg)

- 節點有左右兩邊子樹，左子樹取最大值，右子樹取最小值。
- ![20121027jXNlOV19Yz](https://hackmd.io/_uploads/H1XzxWZ1A.jpg)

# [Day 16 二元搜尋樹Binary Search Tree-實作](https://ithelp.ithome.com.tw/articles/10272982)

## 二元搜尋樹（Binary Search Tree）建立的方法
* insert: 新增元素進入樹中
* delete: 從樹中刪除此元素
* preOrderTraversal: 前序走訪
* inOrderTraversal: 中序走訪
* postOrderTraversal: 後序走訪
* search: 搜尋此元素是否在樹中
---
```JavaScript!
//BST
class Node{
    constructor(data){
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree{
    constructor(){
        this.root = null;
    }
    //插入元素進入樹中
    insert(data){
        let newNode = new Node(data);
        if(this.root === null)
            this.root = newNode;
        else
            this.insertNode(this.root, newNode);
    }
    insertNode(node, newNode){
        if(newNode.data < node.data){
            if(node.left === null)
                node.left = newNode;
            else
                this.insertNode(node.left, newNode);
        }else{
            if(node.right === null)
                node.right = newNode;
            else
                this.insertNode(node.right,newNode);
        }
    }
    //從樹中刪除此元素
    delete(data){
        this.root = this.deleteNode(this.root, data);
    }
    deleteNode(node, data){
        if(node === null){
            return null;
        }else if(data < node.data){
            node.left = this.deleteNode(node.left, data);
            return node;
        }else if(data > node.data){
            node.right = this.deleteNode(node.right, data);
            return node;
        }else{
            if(node.left === null && node.right === null){
                node = null;
                return node;
            }
            if(node.left === null){
                node = node.right;
                return node;
            }
            if(node.right === null){
                node = node.left;
                return node;
            }
            let aux = this.min(node.right);
            node.data = aux.data;
            node.right = this.deleteNode(node.right, aux.data);
            return node;
        }
    }
    min(node=this.root){
        if(node){
            while(node && node.left !== null){
                node = node.left
            }
            return node.data
        }
        return null
    }
    //前序走訪
    preOrderTraversal(){
        let res = [];
        let preHelper=(node)=>{
            if (node) {
                res.push(node.data);
                preHelper(node.left);
                preHelper(node.right);
            }
        }
        preHelper(this.root);
        return res;
    }
    //中序走訪
    inOrderTraversal(){
        let res = [];
        let inHelper=(node)=>{
            if (node) {
                inHelper(node.left);
                res.push(node.data);
                inHelper(node.right);
            }
        }
        inHelper(this.root);
        return res;
    }
    //後序走訪
    postOrderTraversal(){
        const res = [];
        let postHelper=(node)=>{
            if (node) {
                postHelper(node.left);
                postHelper(node.right);
                res.push(node.data);
            }
        }
        postHelper(this.root);
        return res;
    }
    //搜尋此元素是否在樹中
    search(data, node=this.root){
        if(!node){
            return false
        }
        if(data < node.data){
            return this.search(data, node.left)
        }else if(data > node.data){
            return this.search(data, node.right)
        }
        if (data === node.data) {
            return true;
        }
    }
}

let tree = new BinarySearchTree()
tree.insert(11)
tree.insert(7)
tree.insert(15)
tree.insert(5)
tree.insert(3)
tree.insert(9)
console.log("Pre Order Traversal:"+tree.preOrderTraversal())//11,7,5,3,9,15
console.log("In Order Traversal:"+tree.inOrderTraversal())//3,5,7,9,11,15
console.log("Post Order Traversal:"+tree.postOrderTraversal())//3,5,9,7,15,11
tree.delete(9)
console.log(tree.search(9))//false
```

```Python!
#BST
class BinarySearchTree:
    def __init__(self, data=None):
        self.data = data
        self.left = None
        self.right = None

    #插入元素進入樹中
    def insert(self, data):
        if not self.data:
            self.data = data
            return
        if self.data == data:
            return
        if data < self.data:
            if self.left:
                self.left.insert(data)
                return
            self.left = BinarySearchTree(data)
            return
        if self.right:
            self.right.insert(data)
            return
        self.right = BinarySearchTree(data)

    #從樹中刪除此元素
    def delete(self, data):
        if self == None:
            return self
        if data < self.data:
            if self.left:
                self.left = self.left.delete(data)
            return self
        if data > self.data:
            if self.right:
                self.right = self.right.delete(data)
            return self
        if self.right == None:
            return self.left
        if self.left == None:
            return self.right
        aux = self.right
        while aux.left:
            aux = aux.left
        self.data = aux.data
        self.right = self.right.delete(aux.data)
        return self

    #前序走訪
    def preOrderTraversal(self, root):
        res = []
        if root:
            res.append(root.data)
            res = res + self.preOrderTraversal(root.left)
            res = res + self.preOrderTraversal(root.right)
        return res

    #中序走訪
    def inOrderTraversal(self,root):
        res = []
        if root:
            res = self.inOrderTraversal(root.left)
            res.append(root.data)
            res = res + self.inOrderTraversal(root.right)
        return res

    #後序走訪
    def postOrderTraversal(self, root):
        res = []
        if root:
            res = self.postOrderTraversal(root.left)
            res = res + self.postOrderTraversal(root.right)
            res.append(root.data)
        return res

    #搜尋此元素是否在樹中
    def search(self, data):
        if not self.data:
            return False
        if data < self.data:
            if self.left == None:
                return False
            return self.left.search(data)
        if data > self.data:
            if self.right == None:
                return False
            return self.right.search(data)
        if data == self.data:
            return True

tree = BinarySearchTree()
tree.insert(11)
tree.insert(7)
tree.insert(15)
tree.insert(5)
tree.insert(3)
tree.insert(9)
print(tree.preOrderTraversal(tree))#11, 7, 5, 3, 9, 15
print(tree.inOrderTraversal(tree))#3, 5, 7, 9, 11, 15
print(tree.postOrderTraversal(tree))#3, 5, 9, 7, 15, 11
print(tree.search(9))#True
tree.delete(9)
print(tree.search(9))#False
```

# [Day17 堆積Heap](https://ithelp.ithome.com.tw/articles/10273685)

## 最小堆積(Min-Heap)
* 樹根(Root)會是所有節點==最小值==。
* 所有的父節點的值都比子節點小。
![201210274K4qeSzXeV](https://hackmd.io/_uploads/BkVH5YQ1A.jpg)

## 最大堆積(Max-Heap)
樹根(Root)會是所有節點==最大值==。
所有的父節點的值都比子節點大。
![20121027ZIiLFCOt0X](https://hackmd.io/_uploads/ByaY5YXy0.jpg)

## 製作堆積(最大堆積為例)
![20121027YHNMhucWpH](https://hackmd.io/_uploads/rJw6ct710.jpg)

## 新增節點至堆積(最大堆積為例)
插入7至空節點，7比父節點5大，兩者交換。
![201210274904xtgrMf](https://hackmd.io/_uploads/SkSJsFmkR.jpg)

## 刪除節點(最大堆積為例)
預刪除節點4與最後節點5交換後再移除4，接著5跟其他節點作堆積比較。
![20121027H4ARKzGEmG](https://hackmd.io/_uploads/HJhNiYX1R.jpg)

> 堆積可用來製作優先佇列，每個元素有不同優先權，若要取出最大權的元素，可用最大堆積，反之，則用最小堆積。

# [Day18 堆積Heap-實作](https://ithelp.ithome.com.tw/articles/10274633)

## 堆積(Heap)建立的方法(以最大堆積實作)
* maxHeapify: 最大堆積化
* push: 新增元素
* pop: 刪除特定元素
* popRoot: 刪除根節點(最大值)
* getRoot: 查看根節點(最大值)
* printHeap: 印出最大堆積

#### JavaScript
```javascript!
//MaxHeap
class MaxHeap{
    constructor(){
        this.list = [];
    }

    //最大堆積化
    maxHeapify = (arr, n, i) => {
        let largest = i;
        let l = 2 * i + 1;
        let r = 2 * i + 2;

        if (l < n && arr[l] > arr[largest]) {
            largest = l;
        }

        if (r < n && arr[r] > arr[largest]) {
            largest = r;
        }

        if (largest != i) {
            [arr[i],arr[largest]] = [arr[largest],arr[i]];
            this.maxHeapify(arr, n, largest);
        }
    }

    //新增元素
    push = (num) => {
        const size = this.list.length;
        if(size === 0){
            this.list.push(num);
        }else{
            this.list.push(num);
            for (let i = parseInt(this.list.length / 2 - 1); i >= 0; i--) {
                this.maxHeapify(this.list, this.list.length, i);
            }
        }
    }

    //刪除元素
    pop = (num) => {
        const size = this.list.length;
        let i;
        for(i = 0; i < size; i++){
            if(num === this.list[i]){
                break;
            }
        }
        //要刪除元素與最後一個元素交換
        [this.list[i], this.list[size - 1]] = [this.list[size - 1], this.list[i]];

        //刪除最後一個元素
        this.list.splice(size - 1);

        for (let i = parseInt(this.list.length / 2 - 1); i >= 0; i--) {
            this.maxHeapify(this.list, this.list.length, i);
        }
    }

    //回傳最大值
    getRoot = () => this.list[0];

    //刪除最大值
    popRoot = () => {
        this.pop(this.list[0]);
    }

    //印出最大堆積
    printHeap = () => this.list;
}

let heap = new MaxHeap()
heap.push(20)
heap.push(10)
heap.push(5)
heap.push(80)
heap.push(75)
heap.push(78)
heap.push(72)
heap.push(73)
console.log(heap.printHeap())//80, 75, 78, 73, 20, 5, 72, 10
heap.popRoot()
console.log(heap.printHeap())//78, 75, 72, 73, 20, 5, 10
```

#### Python
heapq是能實現Heap結構的套件，Python滿多會使用此種作法，只不過在刪除元素時，似乎只有把最小/大值刪除的方法，這應該是因為用Heap來達到優先佇列(Priority Queue)的需求，權重最高的根節點會優先處理，後進先出的概念。
```python!
#MaxHeap
import heapq

class MaxHeap:
    def __init__(self):
        self.maxheap = []

    def push(self, key):
        heapq.heappush(self.maxheap, key)
        heapq._heapify_max(self.maxheap)

    def getRoot(self):
        return self.maxheap[0]

    def popRoot(self):
        heapq.heappop(self.maxheap)
        heapq._heapify_max(self.maxheap)

    def printHeap(self):
        print(self.maxheap)

heap = MaxHeap()
heap.push(20)
heap.push(10)
heap.push(5)
heap.push(80)
heap.push(75)
heap.push(78)
heap.push(72)
heap.push(73)
heap.printHeap()#80, 75, 78, 73, 20, 5, 72, 10
heap.popRoot()
heap.printHeap()#78, 75, 72, 73, 20, 5, 10
heap.popRoot()
heap.printHeap()#75, 73, 10, 72, 20, 5
```

# [Day19 圖Graph](https://ithelp.ithome.com.tw/articles/10274995)
由==數個頂點Vertex(或稱節點Node)及數條邊(Edge)所構成==，頂點與頂點之間用邊連接，用來表示兩點的關聯與關係。

![20121027fegApxmTst](https://hackmd.io/_uploads/B1NXEcPk0.jpg)

## 一個圖可以用G=( V, E )來定義
* G： 圖形(Graph)
* V： 所有頂點(Vertice)的集合
* E： 所有邊(Edges)的集合

### 同構(Isomorphism)
同構是指若兩個以上的圖，它們頂點與頂點之間的關聯是一致的，就是同構的圖。
![20121027iidfFUQyvO](https://hackmd.io/_uploads/SJy0VcvyC.jpg)
> 上方圖，無論直線或曲線，只要頂點間的關聯一樣，就屬於同構圖。
![20121027aFfrK9yY9G](https://hackmd.io/_uploads/Syb-HcP10.jpg)
> 上方圖，頂點位置被任意移動，只要頂點間的關聯一樣，就屬於同構圖。

## 有向圖(Directed Graph)
具有方向性的邊，表示某個邊是單向的。
![20121027WGIXOggj8G](https://hackmd.io/_uploads/SJTMB5vJ0.jpg)

## 無向圖(Undirected Graph)
不具有方向性的邊，表示某個邊是雙向的。
![20121027hx6jMmTi3A](https://hackmd.io/_uploads/SyB8r9vyR.jpg)

## 加權圖(Weighted Graph)
邊上附有數值稱為邊的「加權」或 「權重」，可以表示出相聯的兩頂點的連接程度，常見的權值應用有: 成本、距離、時間……等。
![20121027msh3olB3We](https://hackmd.io/_uploads/B1IuBcw1R.jpg)

## 圖的術語
![20121027qJQMAlLSxy](https://hackmd.io/_uploads/HkdsIcwyA.jpg)

* 路徑(Path)：兩頂點經過的邊。如上圖:1到6，可以從1到4再到6，可以說(1,4,6)為其路徑。
* 路徑長度(Path Length)：兩頂點路徑上包含邊的個數。如上圖:1到6，路徑為 {(1,4), (4,6)}，經過２條邊。
* 簡單路徑(Simple Path)：路徑上除了起點與終點可以相同外，其餘頂點不能被重複經過。如上圖: 1 4 6為簡單路徑，頂點都未重複。 1 4 3 2 1為簡單路徑，1為起終點，其餘頂點都未重複。 2 3 4 1 3 2不為簡單路徑，其中的頂點3出現2次。 循環(Cycle)：屬於簡單路徑，且路徑的起點與終點相同能構成循環。如上圖:3 2 1 4 3，起點與終點都是3。
* 相鄰(Adjacent)：兩個頂點間有一條邊相連(不論是否具有方向性)，則這兩個頂點為相鄰。如上圖:3與1 2 4相鄰。
* 分支度(Degree)：
    * 無向圖中: 一個頂點含有幾條邊，如下圖:B的分支度為2。
    ![20121027EBWLA2E74t](https://hackmd.io/_uploads/S1EZxqqkR.jpg)
    * 有向圖中: 入分支度與出分支度的總和，如下圖B的分支度為3。
        * 入分支度(In-Degree)： 箭頭指入此頂點的邊數，如下圖B的入分支度為2。
        * 出分支度(Out-Degree)： 箭頭由此頂點指出的邊數，如下圖B的出分支度為1。
    ![201210270BQkQC7aj8](https://hackmd.io/_uploads/r14Xe9q1A.jpg)
* 完整圖(Complete Graph)：
    * 無向圖中: n 個頂點，會有n(n-1) ÷ 2條邊，如下圖: 4個頂點，6條邊。
    ![20121027qQuUDjMyOz](https://hackmd.io/_uploads/ByhUlq51A.jpg)
    * 有向圖中: n 個頂點，會有n(n-1)條邊，如下圖: 4個頂點，12條邊。
    ![20121027BPtSXQTxzW](https://hackmd.io/_uploads/S1W_eccJA.jpg)
* 子圖(Subgraph)：如下圖:乙圖與丙圖的頂點被包含在甲圖的頂點集合中，且乙圖與丙圖的邊也被包含在甲圖的邊集合中，則可以說乙與丙是甲的子圖。
![20121027nWCaKwrtcT](https://hackmd.io/_uploads/SkRtec9JC.jpg)

## 常見圖的表示法

### 相鄰矩陣 (Adjacency Matrix)
一個包含N個頂點的圖，可以使用 N 乘 N 的二維陣列來表示，兩個頂點間，若有邊值為1；若沒有邊值為0。

* 無向圖中:
![20121027nSWkwuvrWg](https://hackmd.io/_uploads/rJPX-951A.jpg)
==矩陣會是對稱的==，如上圖G[2][3] = G[3][2] = 1，只需保存==上三角==或==下三角部==份即可，n(n-1)/2的記憶體空間。
==分支度==: 該頂點在矩陣中列的總和，如上圖：頂點2的分支度 = 1 + 0 + 1 + 0 = 2

* 有向圖中:
![20121027BWCQd6MYjs](https://hackmd.io/_uploads/rJ8XDqcy0.jpg)
==矩陣不一定會是對稱的==，如上圖G[1][3]=1，G[3][1]=0。
==分支度==: 該頂點入分支度與出分支度的總和。
==出分支度==： 第i列的總和，如上圖的頂點3，出分支度為第3列的總和為 0。
==入分支度==： 第i行的總和，如上圖的頂點3，入分支度為第3行的總和 1 + 1 + 0 = 2

## 相鄰串列 (Adjacency List)
每個頂點使用==單向鏈結串列來串接相鄰的頂點==，==只處理有邊的部分，沒有邊的部分不處理==，有N個頂點，需準備N個串列，再用指標指向相鄰頂點。

* 無向圖中:
![20121027SyzgwBrqnc](https://hackmd.io/_uploads/BJohD59kR.jpg)
若有n個頂點，e條邊，則需要 ==n 個首節點與 2乘e 個子節點==，如上圖有4個頂點，與4條邊，則需要4個首節點與8個子節點。
==分支度==: 該頂點串列子節點的總數，如上圖:頂點1的串列，除了首節點之外，有3個子節點，分支度為 3

* 有向圖中:
![20121027yvfeq2zquC](https://hackmd.io/_uploads/Bybk_qcyR.jpg)
若有n個頂點，e條邊，則需要 ==n 個首節點與 e 個子節點==，如上圖有3個頂點，與4條邊，則需要3個首節點與4個子節點。
出分支度： 該頂點串列子節點的總數，如上圖:頂點1的串列，除了首節點之外，有2個子節點，出分支度為 2。
入分支度： 比較複雜，需要尋找其他頂點串列的子節點，是否出現該頂點，再加總。
>相鄰矩陣在路經很少的情況下容易浪費空間，但求分支度就非常方便，而相鄰串列法比較節省空間，但求分支度時比較麻煩。

# [Day20 圖Graph-實作](圖Graph-實作)

## 圖(Graph)建立的方法
* addVertex: 新增頂點
* addEdge: 新增邊
* removeVertex: 刪除頂點
* removeEdge: 刪除邊

#### JavaScript
* 無向圖
* 相鄰串列 (Adjacency List
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
```
![20121027CE7B6iEvM0](https://hackmd.io/_uploads/BJrGdpggR.jpg)

#### Python
* 無向圖
* 使用Numpy實作相鄰矩陣(Adjacency Matrix)
```python=
import numpy as np

vertices = {0, 1, 2, 3, 4}
edges = {(0, 1), (1, 2), (0, 3), (1, 3), (3, 4)}

adjacencyMatrix = np.zeros((5, 5)).astype(int)

for edge in edges:
    v1 = edge[0]
    v2 = edge[1]
    adjacencyMatrix[v1][v2] = 1
    adjacencyMatrix[v2][v1] = 1

print("Vertices:",vertices)
print("Edges:",edges)
print(adjacencyMatrix)

#Vertices: {0, 1, 2, 3, 4}
#Edges: {(0, 1), (1, 2), (1, 3), (0, 3), (3, 4)}
#[[0 1 0 1 0]
# [1 0 1 1 0]
# [0 1 0 0 0]
# [1 1 0 0 1]
# [0 0 0 1 0]]
```
![20121027LrGXUEJeNB](https://hackmd.io/_uploads/S1wE_6ee0.jpg)
