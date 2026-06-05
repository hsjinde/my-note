```python!
class TreeNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False
class WordDictionary:

    def __init__(self):
        self.root = TreeNode()

    def addWord(self, word: str) -> None:
        cur = self.root 
        for w in word:
            if w not in cur.children:
                cur.children[w] = TreeNode()
            cur = cur.children[w]
        cur.endOfWord = True

    def search(self, word: str) -> bool:
        def dfs(j, root):
            cur = root 
            for i in range(j, len(word)):
                if word[i] == ".":
                    for child in cur.children.values():
                        if dfs(i+1, child):
                            return True
                    return False
                else:
                    if word[i] not in cur.children:
                        return False
                    cur = cur.children[word[i]]
            return cur.endOfWord
        return dfs(0, self.root)
```

這題的關鍵在searchfunction 當word當中的'.'可以代表任何英文字母，因此遇到'.'時，我們會搜尋所有

#### The complexity of creating a trie is $O(W*L)$
where W is the number of words, and L is an average length of the word: you need to perform L lookups on the average for each of the W words in the set.

#### Space complexity: O(26^n) for our DFS stack.