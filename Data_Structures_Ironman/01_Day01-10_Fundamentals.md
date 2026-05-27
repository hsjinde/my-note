# [Day 1 資料結構 + 演算法](https://ithelp.ithome.com.tw/articles/10262990)

## 資料結構
電腦在儲存資料時，會儲存在電腦的記憶體中，而資料可以有不同的儲存與組織方式，即為資料結構。

## 演算法
演算法被定義為一個可以用來解決某一問題的方法或算法
Donald Ervin Knuth提出了演算法必須具備的五個特性:
1. 輸入(Input): 0個或是1以上的輸入。
2. 輸出(Output): 至少有一個輸出結果。
3. 明確性(Definiteness): 每個步驟或指令必須是明確的而不含糊的。
4. 有限性(Finiteness): 在有限的步驟後一定會結束，不會有無窮迴圈。
5. 有效性(Effectiveness): 每個步驟清楚具有可行性，能用紙筆來表達。

## 複雜度
**關於時間複雜度(Time Complexity)**
可以說電腦執行演算法所花費的時間成本。花費的時間並非以秒為單位計算，是以執行的次數來做計算。
**關於空間複雜度(Space Complexity)**
可以說是電腦執行演算法所需要耗費的記憶體空間。

![](https://hackmd.io/_uploads/HJenAmxCT.jpg)

# [Day 2 陣列Array](https://ithelp.ithome.com.tw/articles/10263001)
陣列(Array)是一種常見的資料結構，常用來處理相同類型的有序資料，並存放在連續的記憶體空間中。

- 宣告固定記憶體空間，容易造成記憶體浪費
- 讀取與修改資料時較快速 (可以透過索引值index快速找到要的資料位置)

![](https://hackmd.io/_uploads/HJLsbmWRT.jpg)

- 追加或刪除資料時較費時間
- 若要插入的位置已有資料，需先依序將它們移位再插入

追加:
![](https://hackmd.io/_uploads/SyVvM7WCT.jpg)
刪除:
![](https://hackmd.io/_uploads/B19vM7b0p.jpg)

JavaScript:
```javascript!
//Array
//一維陣列
var a = [1,2,3]
console.log(a[1]) //2
//二維陣列
var b = [[1,2],[3,4],[5,6]]
console.log(b[0][1]) //2
//三維陣列
var c = [[[1,2,3],[4,5,6],[7,8,9]]]
console.log(c[0][1][2]) //6
```
> JavaScript本身就帶有Array資料型態，詳細使用方法可以參考[這篇](https://ithelp.ithome.com.tw/articles/10213787)介紹。

python:
```python!
#Array
import numpy as np
#一維陣列
a = np.array([1,2,3])
print(a[2]) //3
#二維陣列
b = np.array([[1,2,3],[4,5,6]])
print(b[0][1]) //2
#三維陣列
c = np.array([[[1,2,3],[4,5,6],[7,8,9]]])
print(c[0][1][2]) //6
```

# [Day 3 鏈結串列Linked List](https://ithelp.ithome.com.tw/articles/10263840)
鏈結串列(Linked List)常用來處理相同類型資料，在==不連續==的記憶體位置，以隨機的方式儲存，由於不用事先宣告一塊連續記憶體空間，所以較不會造成記憶體的浪費。

由許多節點組成，每個節點包含==資料欄==與==指標欄==，指標欄會指向下一個資料所在的記憶體位置。因此再追加或刪除資料相當方便，因為只需要==更動指標的指向==，但在讀取資料就會較費時，因為必須從串列的頭開始尋找。

## 單向鏈結串列Singly Linked List
基本的鏈結串列結構，從串列頭(Head)開始依序指向下一筆資料，串列尾(Tail)為最後一筆資料指向空值(Null)。

![20121027KfaEdl6AVY](https://hackmd.io/_uploads/SkxqiEQR6.jpg)

### Single link list的一些基本操作
```pyhon=
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```
1. 求length(Node總數)
```pyhon=
def length(self):
    current_node = self.head
    length = 0
    while current_node:
        length += 1
        current_node = current_node.next
    return length
```
2. 反轉
```pyhon=
def reverse(self):
    prev = None
    current = self.head
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    self.head = prev
```
![linkedlist-reverse-iterative](https://hackmd.io/_uploads/SkD-tUBCT.png)

## 雙向鏈結串列Doubly Linked List
每個節點會變成==一個資料欄==與==兩個指標欄==，讓資料可以從頭或尾巴開始找，優點是可以讓被破壞或遺失的節點被找回來，但在追加或刪除資料時，必須更動==比單向鏈結串列更多的指標次數==。

![20121027GQdhacYTQ0](https://hackmd.io/_uploads/HkjUMB7R6.jpg)

## 環狀鏈結串列 Circular Linked List
在單向鏈結串列中，萬一某節點被破壞或遺失，整個串列將會沒作用，因此如果把串列最後一個節點指向串列頭，這樣==每個節點都可以是串列頭==。

![20121027lkG5jI8XoZ](https://hackmd.io/_uploads/BykFGr7Cp.jpg)

## 陣列與鏈結串列的優缺點比較
![20121027POuiLls7tO](https://hackmd.io/_uploads/ryS5fr7Aa.jpg)

# [Day4 鏈結串列Linked List-實作](https://ithelp.ithome.com.tw/articles/10264368)

## 鏈結串列(Linked List)建立的方法
* append: 在尾部新增節點
* insertAt: 在特定位置插入節點
* removeAt: 刪除特定位置節點
* remove: 刪除特定資料節點
* indexOf: 回傳第一個出現指定資料的節點位置，空值則回傳-1
* isEmpty: 確認是否為空串列
* size: 串列的長度
* print: 印出串列所有資料

### JavaScript(單向串列為例)
```javascript!
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }
  //在尾部插入節點
  append(data) {
    let newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while(current.next){
          current = current.next
      }
      current.next = newNode
    }
    this.length += 1;
  }
  //特定位置插入節點
  insertAt(index, data) {
    // 確認給的位置是否超出實際總數。
    if (index < 0 || index >= this.length) {
      return null;
    }
    let newNode = new Node(data);
    let current = this.head;
    let previous;
    let count = 0;
    if (index == 0) {
      this.head = newNode;
      newNode.next = this.head;
    } else {
      while(count != index){
          count ++;
          previous = current;
          current = current.next;
      }
      newNode.next = current;
      previous.next = newNode;
    }
    this.length += 1;
  }
  //刪除特定位置節點
  removeAt(index) {
    if (index < 0 || index >= this.length){
        return false;
    }
    let current = this.head;
    let previous;
    let count = 0;
    if (index === 0) {
      this.head = current.next;
    } else {
      while(count != index){
          count ++;
          previous = current;
          current = current.next;
      }
      previous.next = current.next
    }
    this.length -= 1;
  }
  //刪除特定節點
  remove(data) {
    let current = this.head;
    let previos = null;
    while (current != null) {
      if (current.data === data) {
        if (previos == null) {
          this.head = current.next;
        } else {
          previos.next = current.next;
        }
        this.length -= 1;
      }
      previos = current;
      current = current.next;
    }
  }
  //回傳第一個出現此資料節點的位置，若空值則回傳-1
  indexOf(data) {
    let currNode = this.head;
    let count = 0;
    while (currNode) {
      if (currNode.data === data) {
        return count;
      }
      count += 1;
      currNode = currNode.next;
    }
    return -1;
  }
  //是否為空串列
  isEmpty() {
    return this.length === 0;
  }
  //串列長度
  size() {
    return this.length;
  }
  //印出串列所有資料
  print() {
    const temp = [];
    let currNode = this.head;
    while (currNode) {
      temp.push(currNode.data);
      currNode = currNode.next;
    }
    return temp.join(', ');
  }
}

let ll = new LinkedList();
console.log(ll.isEmpty());//true
ll.append(10);
ll.append(20);
console.log(ll.isEmpty());//false
console.log(ll.print())//"10, 20"
ll.insertAt(1,60);
console.log(ll.print())//"10, 60, 20"
ll.append(20);
console.log(ll.print())//"10, 60, 20, 20"
ll.removeAt(2)
console.log(ll.print())//"10, 60, 20"
ll.remove(20)
console.log(ll.size())//2
console.log(ll.print())//"10, 60"
```

### Python(單向鏈結串列為例)
```python!
class Node:
    def __init__(self, data=None, next=None):
        self.data = data
        self.next = next

class LinkedList:
    def __init__(self, head=None):
        self.head = head

    def append(self, data):
        if not self.head:
            self.head = Node(data)
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = Node(data)

    def insertAt(self, index, data):
        if index < 0 or index >= self.size():
            return
        node = Node(data)
        current = self.head
        previous = None
        count = 0
        if index == 0:
            self.head = Node(data, node)
        else:
            while count != index:
                count += 1
                previous = current
                current = current.next
            new_node = Node(data, previous.next)
            previous.next = new_node

    def removeAt(self, index):
        if index < 0 or index >= self.size():
            return
        current = self.head
        previous = None
        count = 0
        if index == 0:
            self.head = current.next
        else:
            while count != index:
                count += 1
                previous = current
                current = current.next
            previous.next = current.next

    def remove(self, data, all=False):
        current = self.head
        previous = None
        while current:
            if current.data == data:
                if previous:
                    previous.next = current.next
                    current.next = current
                else:
                    self.head = current.next
                if not all:
                    return
            else:
                previous = current
            current = current.next

    def indexOf(self, data):
        node = self.head
        count = 0
        while node:
            if node.data == data:
                return count
            count += 1
            node = node.next

    def isEmpty(self):
        return self.head is None

    def size(self):
        count = 0
        node = self.head
        while node:
            count += 1
            node = node.next
        return count

    def print(self):
        if not self.head:
            print(self.head)
        node = self.head
        while node:
            end = " -> "
            print(node.data, end=end)
            node = node.next

ll = LinkedList()
print(ll.isEmpty())#True
ll.append(10)
ll.append(20)
print(ll.isEmpty())#False
print(ll.print())#10 -> 20 -> None
ll.insertAt(1,60)
print(ll.print())#10 -> 60 -> 20 -> None
ll.append(20)
print(ll.print())#10 -> 60 -> 20 -> 20 -> None
ll.remove(20)
print(ll.print())#10 -> 60 -> 20 -> None
ll.removeAt(1)
print(ll.print())#10 -> 20 -> None
ll.remove(20)
print(ll.size())#1
print(ll.print())#10 -> None
```

# [Day5 堆疊Stack](https://ithelp.ithome.com.tw/articles/10265265)
任何動作都必須從最頂端(top)進行，因此有「後進先出」(Last In First Out)特性，縮寫為LIFO。
**在應用中:**
* 回到上一頁: 最先瀏覽的頁面，會在點擊最後一次此按鈕時才出現。
* 還原: 最先被記錄的動作，會在點擊最後一次此按鈕時才被還原。

![20121027edOmUwqfoe](https://hackmd.io/_uploads/HysfXES06.jpg)

使用陣列(Array)製作堆疊
![20121027cRE4REw4KL](https://hackmd.io/_uploads/HkXUQNSA6.jpg)

使用鏈結串列(Linked List)製作堆疊
![20121027lorqGb2vXF](https://hackmd.io/_uploads/SyoIQ4SAT.jpg)

# [Day6 堆疊Stack-實作](https://ithelp.ithome.com.tw/articles/10266064)
* push: 新增元素
* pop: 從頂端移除元素
* peek: 查看頂端(top)元素
* size: 查看此堆疊的元素量

### JavaScript(使用陣列):
```javascript!
//Stack
class Stack {
  constructor(){
    this.list = []
  }
  // 新增元素
  push(data) {
    this.list.push(data)
  }
  // 從頂端移除元素
  pop(){
    return this.list.pop()
  }
  // 此堆疊元素量
  size(){
    return this.list.length;
  }
  // 查看最頂端元素
  peek(){
    return this.list[this.list.length - 1]
  }
}

let stack = new Stack()
stack.push(20)
stack.push(60)
console.log(stack.size())//2
console.log(stack.peek())//60
stack.pop()
console.log(stack.peek())//20
```

### JavaScript(使用鏈結串列)
```javascript!
class StackNode {
  constructor(data) {
    this.data = data
    this.next = null
  }
}

class LinkedStack {
  constructor() {
    this.top = null
    this.length = 0
  }
  // 新增元素
  push(data) {
    let node = new StackNode(data)
    if(!this.top){
      this.top = node
      this.length = 1
    }else{
      let current = new StackNode(data)
      current.next = this.top
      this.top = current
      this.length+=1
    }
  }
  // 刪除頂端元素
  pop() {
    if(!this.top){
      return null
    }else{
      this.top = this.top.next
      this.length -= 1
    }
  }
  // 查看最頂端元素
  peek() {
    if(!this.top){
      return null
    }else{
      return this.top.data
    }
  }
  // 此堆疊元素量
  size() {
    return this.length
  }
}

let stack = new LinkedStack()
stack.push('20')
stack.push('30')
stack.push('40')
console.log(stack.size())//3
console.log(stack.peek())//"40"
stack.pop()
console.log(stack.peek())//"30"
console.log(stack.size())//2
stack.pop()
console.log(stack.size())//1
console.log(stack.peek())//"20"
```

### Python(使用鏈結串列)
```python!
class StackNode():
    def __init__(self, data=None, next=None):
        self.data = data
        self.next = None

class LinkedStack():
    def __init__(self, top=None):
        self.top = top

    def push(self, data):
        if self.top is None:
            self.top = StackNode(data)
        else:
            current = StackNode(data)
            current.next = self.top
            self.top = current

    def pop(self):
        if self.top is None:
            return None
        else:
            self.top = self.top.next
            return

    def peek(self):
        if self.top is None:
            return None
        return self.top.data

    def size(self):
        count = 0
        current = self.top
        while current:
            count += 1
            current = current.next
        return count

stack = LinkedStack()
stack.push('20')
stack.push('30')
stack.push('40')
print(stack.size())#3
print(stack.peek())#"40"
stack.pop()
print(stack.peek())#"30"
print(stack.size())#2
stack.pop()
print(stack.size())#1
print(stack.peek())#"20"
```

# Day7 佇列Queue
佇列(Queue)是一種排列結構，雖然與堆疊類似，但佇列在新增與刪除資料必須在不同端進行，前端(front)能夠刪除(dequeue)與查看(peek)資料，尾端(Rear)只能新增(enqueue)資料，因此有「先進先出」(First In First Out)特性，縮寫為==FIFO==。

### Enqueue的情況
![20121027OU2QKm6j55](https://hackmd.io/_uploads/ryZal2w0a.jpg)
### Dequeue的情況
![201210276YJ4rjLvqG](https://hackmd.io/_uploads/SkrAghDAa.jpg)

## 常見製作佇列的方式
* 使用陣列(Array)製作佇列
* 使用鏈結串列(Linked List)製作佇列

# Day8 佇列Queue-實作

## 佇列(Queue)建立的方法
* enqueue: 尾端新增元素
* dequeue: 從前端移除元素
* peek: 查看最前端元素
* size: 此佇列的元素量

### JavaScript(使用陣列)
```javascript!
//Queue
class Queue {
  constructor(){
    this.list = []
  }
  // 尾端新增元素
  enqueue(data){
    this.list.push(data)
  }
  // 從前端移除元素
  dequeue(){
    this.list.shift();
  }
  // 查看最前端元素
  peek(){
    return this.list[0]
  }
  // 此佇列的元素量
  size(){
    return this.list.length;
  }
}

let queue = new Queue()
queue.enqueue(20)
queue.enqueue(30)
queue.enqueue(40)
console.log(queue.size())//3
console.log(queue.peek())//20
queue.dequeue()
console.log(queue.peek())//30
```

### JavaScript(使用鏈結串列)
```javascript!
class QueueNode {
  constructor(data) {
    this.data = data
    this.next = null
  }
}

class LinkedQueue {
  constructor() {
    this.front = null
    this.rear = null
    this.length = 0
  }
  // 從尾端新增元素
  enqueue(data) {
    let temp = new QueueNode(data);
    if (this.rear == null) {
      this.front = temp;
      this.rear = temp;
    } else {
      this.rear.next = temp;
      this.rear = temp;
    }
    this.length += 1
  }
  // 從前端移除元素
  dequeue() {
    if (!this.front) return null
    if (this.front === this.rear) {
      this.rear = null
    }
    this.front = this.front.next
    this.length -= 1
  }
  // 查看最前端元素
  peek() {
    return this.front.data
  }
  // 此佇列的元素量
  size() {
    return this.length
  }
}

let queue = new LinkedQueue()
queue.enqueue('20')
queue.enqueue('30')
queue.enqueue('40')
console.log(queue.peek()) //"20"
console.log(queue.size()) //3
queue.dequeue()
console.log(queue.peek()) //"30"
console.log(queue.size()) //2
queue.dequeue()
console.log(queue.peek()) //"40"
console.log(queue.size()) //1
queue.dequeue()
console.log(queue.peek()) //null
```

### Python(使用鏈結串列)
```python!
class QueueNode():
    def __init__(self, data=None, next=None):
        self.data = data
        self.next = next

class LinkedQueue():
    def __init__(self, front=None, rear=None):
        self.front = front
        self.rear = rear

    def enqueue(self, data):
        if self.front is None:
            self.front = QueueNode(data)
            self.rear = self.front
        else:
            self.rear.next = QueueNode(data)
            self.rear = self.rear.next

    def dequeue(self):
        if self.rear is None:
            return None
        if self.front == self.rear:
            self.rear = None
        self.front = self.front.next

    def peek(self):
        if self.front is None:
            return None
        return self.front.data

    def size(self):
        count = 0
        current = self.front
        while current:
            count += 1
            current = current.next
        return count

queue = LinkedQueue()
queue.enqueue('20')
queue.enqueue('30')
queue.enqueue('40')
print(queue.peek())#20
print(queue.size())#3
queue.dequeue()
print(queue.peek())#30
print(queue.size())#2
queue.dequeue()
print(queue.peek())#40
print(queue.size())#1
queue.dequeue()
print(queue.peek())#None
print(queue.size())#0
```

# Day9 雜湊表Hash Table
雜湊表(Hash Table)又稱哈希表，是透過雜湊函式(Hash Function)來計算出一個鍵(key)與值(value)所對應的位置，進而建立雜湊表格，而後也能夠過雜湊函式來搜尋找出鍵值存放在表格的位置。
由於動作都需要先經由雜湊函式來執行，若不被知道雜湊函式情況下，保密性就極高，因此很常被應用在:
* 加密
* 解密
* 壓縮
* 驗證

![20121027auspOHzYk5](https://hackmd.io/_uploads/HynWa-t0p.jpg)

## 雜湊表一些專有名詞
* 桶(Bucket):雜湊表中儲存資料的位置，每一個位置對應唯一位址(Bucket Address)。
* 槽(Slot):每一個桶中的資料欄位，例如：一筆資料有2個欄位，則代表桶中有2個槽。
* 碰撞(Collision):若2筆資料經過雜湊函數運算後的雜湊值相同，也就是對應到相同位址時，稱為碰撞。
* 溢位(Overflow):資料經過雜湊函數運算後，雜湊值所指向的桶位址已滿，無法再儲存，稱為溢位。

## 雜湊函式設計原則
* 盡可能降低碰撞與溢位的產生
* 不宜過於複雜，越容易計算越佳
* 盡可能把文字轉換成數字的鍵值
* 得到的雜湊值，盡可能分布均勻在每個桶中，不要過於集中。

## 常見的雜湊函式(Hash Function)
### 除法(Mod/Division)
相除取餘數來當作雜湊值。
例如:有11個Bucket，若有數值4。
4 mod 11 = 4，雜湊值為4。
### 中間平方法(Middle Square)
將值平方後，再取適當的中間位數作為雜湊值。
例如:有1000個Bucket，若有數值235。
235 X 235 = 55225，取中間三位數，雜湊值為522。
### 折疊相加法(Folding Addition)
相加方式分為兩種:
Shift(移位)
例如:一個大數值987586265，拆分成3段相加，987+586+265，雜湊值為1838。
Boundary(邊界)
例如:一個大數值987586265，拆分成3段，可分為基數段反轉或偶數段反轉，偶數段反轉為:
987+685+265，雜湊值為1937。
### 數位分析法(Digits Analysis)
假設欄位資料已知，又屬於同一位數，若很集中則捨棄該位，若很分散則挑選該位。
例如: 手機號碼欄位，都是09開頭+8位號碼，捨去09，取用8位號碼作為雜湊值。

## 碰撞(Collision)與溢位(Overflow)常見的處理方式
### 線性探測法(Linear Probing)
假設2筆資料得出一樣的雜湊值，將以線性方式往後尋找直到有空的Bucket為止，一般來說也會視為環狀結構，若後面Bucket都滿了，可以循環到前面尋找。
![20121027ScabqSVxft](https://hackmd.io/_uploads/rkfKJzFAT.jpg)
### 平方探測法(Quadratic Probing)
透過 (H(x) ± i²) mod b，b為bucket數，1 ≤ i ≤ (b-1)/2，用此公式去尋找其他有空的Bucket。
第一次尋找: (H(x) + 1²) mod b
第二次尋找: (H(x) - 1²) mod b
第三次尋找: (H(x) + 2²) mod b
第四次尋找: (H(x) - 2²) mod b…...以此類推。
![20121027YslMDxMQbP](https://hackmd.io/_uploads/HyDpyGKA6.jpg)
### 鏈結法(Chaining)
使用鏈結串列(Linked List)資料結構在每一組Bucket中。
![20121027hdzcEEqGsx](https://hackmd.io/_uploads/Bk1jjGKR6.jpg)

## 再雜湊法(Rehashing)
準備多個雜湊函式，當一個發生溢位時，則使用第二個函式，以此類推。
> 雜湊表初始的陣列規模若太小，容易造成碰撞次數增加，而需要多次的碰撞處理;若規模太大，容易造成過多未儲存數據的陣列空間，因此初始設定適當的陣列規模相當重要。

# Day 10 雜湊表Hash Table-實作

## 雜湊表(Hash Table)建立的方法
* hash: 雜湊函式
* add: 新增資料
* search: 搜尋資料
* remove: 刪除資料

## 本實作使用
* 使用==除法雜湊函式==將每個字元轉換成unicode碼加總/Bucket數量，再==取餘數==。
* 使用==Chaining==方式來處理==碰撞(Collision)==。

**JavaScript:**
```javascript!
// 雜湊函式
function hash(key, size) {
  let hashCode = 0;
  for (let index in key) {
    hashCode += key.charCodeAt(index);
  }
  return hashCode % size;
}

// 鏈結串列節點
class Node {
  constructor(key, value) {
    this[key] = value;
    this.next = null;
  }
}

// 鏈結串列
class LinkedList {
  constructor(node) {
    this.head = node;
    this.count = 0;
  }
}

// 雜湊表
class HashTable {
  constructor() {
    this.size = 5;
    this.values = new Array(this.size).fill(null);
    this.length = 0;
  }

  // 新增資料
  add(key, value) {
    const hashIndex = hash(key, this.size);
    const node = new Node(key, value);

    if (!this.values[hashIndex]) {
      this.values[hashIndex] = new LinkedList(node);
    } else {
      let current = this.values[hashIndex].head;
      while (current.next) {
        current = current.next;
      }
      current.next = node;
    }
    this.values[hashIndex].count++;
    this.length++;
  }

  // 查詢資料
  search(key) {
    const index = hash(key, this.size);
    const slot = this.values[index];
    let current = slot.head;
    if (current.hasOwnProperty(key)) {
      return current[key];
    }
    while (current.next) {
      if (current.next.hasOwnProperty(key)) {
        return current.next[key];
      } else {
        current = current.next;
      }
    }
    return "Data not found";
  }

  // 刪除資料
  remove(key) {
    const index = hash(key, this.size);
    const slot = this.values[index];
    let current = slot.head;
    if (current.hasOwnProperty(key)) {
      slot.head = current.next;
      slot.count--;
      this.length--;
      return "Data was deleted successfully";
    }
    while (current.next) {
      if (current.next.hasOwnProperty(key)) {
        current.next = current.next.next;
        slot.count--;
        this.length--;
        return "Data was deleted successfully";
      } else {
        current = current.next;
      }
    }
    return "Data is not exhausting";
  }
}

const ht = new HashTable();
ht.add("John", "111-111-111");
ht.add("Taylor", "222-222");
ht.add("Krish", "333-333");
ht.add("Mack", "444-444");
ht.add("Den", "555-555");
ht.add("Mike", "666-666");
ht.add("John", "77-77");
ht.add("Jack", "88-88-88");
ht.add("Jimmy", "99-99");
ht.add("Harry", "121-121");
ht.add("Meet", "232-232");
ht.add("Miraj", "454-454");
ht.add("Milan", "567-567");

console.log(ht.search("Den"));//555-555
console.log(ht.search("Miraj"));//454-454
console.log(ht.search("Drupel"));//Data not found
console.log(ht.remove("Krish"));//Data was deleted successfully
console.log(ht.remove("Mack"));//Data was deleted successfully
console.log(ht.remove("Meet"));//Data was deleted successfully
console.log(ht.remove("Taylor"));//Data was deleted successfully
console.log(ht.remove("JsMount"));//Data is not exhausting
console.log(ht.values);
/*
[
  {
    "head": {
      "Mike": "666-666",
      "next": null
    },
    "count": 1
  },
  null,
  {
    "head": {
      "Jack": "88-88-88",
      "next": {
        "Milan": "567-567",
        "next": null
      }
    },
    "count": 2
  },
  {
    "head": {
      "Jimmy": "99-99",
      "next": {
        "Harry": "121-121",
        "next": null
      }
    },
    "count": 2
  },
  {
    "head": {
      "John": "111-111-111",
      "next": {
        "Den": "555-555",
        "next": {
          "Miraj": "454-454",
          "next": null
        }
      }
    },
    "count": 3
  }
]
*/
```

**Python**
```python!
#Hash Table
class Node:
    def __init__(self, key=None, data=None):
        self.value = {}
        self.value[key] = data
        self.next = None

    def __repr__(self):
        return str(self.data)

class LinkedList:
    def __init__(self, head=None):
        self.head = head
        self.next = None
        self.count = 0

    def __str__(self):
        str_list = []
        current = self.head
        while current:
            str_list.append(str(current.value))
            current = current.next
        return "[" + "->".join(str_list) + "]"

    def __repr__(self):
        return str(self)

class HashTable:
    def __init__(self, size):
        self.size = size
        self.values = [None] * size
        self.length = 0

    def hash(self, key, size):
        hashCode = 0
        for i in range(len(key)):
            hashCode += ord(key[i]) #返回對應ASCll
        return hashCode % size

    def add(self, key, value):
        hashIndex = self.hash(key, self.size)
        node = Node(key, value)

        if not self.values[hashIndex]:
            self.values[hashIndex] = LinkedList(node)
        else:
            current = self.values[hashIndex].head
            while current.next:
                current = current.next
            current.next = node
        self.values[hashIndex].count +=1
        self.length +=1

    def search(self, key):
        hashIndex = self.hash(key, self.size)
        slot = self.values[hashIndex]
        current = slot.head
        if key in current.value:
            return current.value
        while current.next:
            if key in current.next.value:
                return current.next.value
            else:
                current = current.next
        return "Data not found"

    def remove(self, key):
        hashIndex = self.hash(key, self.size)
        slot = self.values[hashIndex]
        current = slot.head
        if key in current.value:
            current = current.next
            slot.count -=1
            self.length -=1
            return "Data was deleted successfully"
        while current.next:
            if key in current.next.value:
                current.next = current.next.next
                slot.count -=1
                self.length -=1
                return "Data was deleted successfully"
            else:
                current = current.next
        return "Data is not exhausting"

    def __repr__(self):
        return str(self.values)

ht = HashTable(5)
ht.add("John", "111-111-111")
ht.add("Taylor", "222-222")
ht.add("Krish", "333-333")
ht.add("Mack", "444-444")
ht.add("Den", "555-555")
ht.add("Mike", "666-666")
ht.add("Jack", "88-88-88")
ht.add("Jimmy", "99-99")
ht.add("Harry", "121-121")
ht.add("Meet", "232-232")
ht.add("Miraj", "454-454")
ht.add("Milan", "567-567")

print(ht)
#[
# [{'Taylor': '222-222'}->{'Mack': '444-444'}->{'Mike': '666-666'}->{'Meet': '232-232'}],
# None,
# [{'Jack': '88-88-88'}->{'Milan': '567-567'}],
# [{'Krish': '333-333'}->{'Jimmy': '99-99'}->{'Harry': '121-121'}],
# [{'John': '111-111-111'}->{'Den': '555-555'}->{'Miraj': '454-454'}]
# ]

print(ht.search('Den'))#{'Den': '555-555'}
print(ht.search('Jimmy'))#{'Jimmy': '99-99'}
print(ht.remove('Den'))#Data was deleted successfully
print(ht.search('Den'))#Data not found
print(ht)
#[
# [{'Taylor': '222-222'}->{'Mack': '444-444'}->{'Mike': '666-666'}->{'Meet': '232-232'}],
# None,
# [{'Jack': '88-88-88'}->{'Milan': '567-567'}],
# [{'Krish': '333-333'}->{'Jimmy': '99-99'}->{'Harry': '121-121'}],
# [{'John': '111-111-111'}->{'Miraj': '454-454'}]
# ]
```
