首先知道 Trie 是一種特殊的 Tree

目前需要儲存所有字元是小寫英文 a-z 組成的字串

為了節省空間所以會是已字元為單位來做存儲

另外是可以透過把 字元當成每個 node 的 edge 上 key

![JTyTsFg](https://hackmd.io/_uploads/BkOkI67xR.png)

每個結點都可以透過 key 來找到下一個結點

這個結構可以使用 hashmap 來實作

而沒每個結點除了這個用來紀錄 child 對應的 hashmap 外

需要一個布林值來紀錄這個結點是否為最後一個結點

如上圖的 Trie 雖然有 appl 的 prefix 但是卻沒有 appl 這個字

```python!
class TreeNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False


class Trie:

    def __init__(self):
        self.root = TreeNode()

    def insert(self, word: str) -> None:
        cur = self.root
        for w in word:
            if w not in cur.children:
                cur.children[w] = TreeNode()
            cur = cur.children[w]  
        cur.endOfWord = True

    def search(self, word: str) -> bool:
        cur = self.root
        for w in word:
            if w not in cur.children:
                return False
            cur = cur.children[w]  
        return cur.endOfWord

    def startsWith(self, prefix: str) -> bool:
        cur = self.root
        for w in prefix:
            if w not in cur.children:
                return False
            cur = cur.children[w] 
        return True 

```

#### time complexity: $O(WL)$
where W is the number of words, and L is an average length of the word. This is because, for each word, we are traversing down the path that corresponds to each character in the word, so in total
#### space complexity: $O(WL)$
since we are storing all the W words of average length L in the Trie