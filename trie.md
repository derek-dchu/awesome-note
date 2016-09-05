# Trie

### Trie Node
We define Trie as a tree structure while each node uses a dict to store edges (We can also use a char array with 256 length for ASCII encoding, but usually it is not a complete tree so using a dict will potentially save some memory).

```python
class TrieNode:
    def __init__(self):
        self.chilren = {}
        self.isWord = False
```

Here we uses a flag: `isWord` to indicates that is there a word ending with current node.

### Trie
A trie a tree structure that contains many `TrieNode` which supports `insert`, `find`, and `findPrefix`. A special implementation may also support removal.

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        if word == '':
            self.root.isWord = True
         
        ptr = self.root
        for c in word:
            if c not ptr.children:
                ptr.children[c] = TrieNode()
            ptr = ptr.children[c]
        ptr.isWord = True
        
    def find(self, word)：
        if word == '':
            return self.root.isWord
        
        ptr = self.root
        for c in word:
            if c in ptr.children:
                ptr = ptr.children[c]
            else:
                return False
        return ptr.isWord

    def findPerfix(self, prefix):
        if prefix == '':
            return self.root.isWord
        
        ptr = self.root
        for c in prefix:
            if c in ptr.children:
                ptr = ptr.children[c]
            else:
                return False
        return True
```

#### Support Removal
To support removal, we need to add another flag `hasWord` within the `TrieNode` which indicates if the node is still contains prefix. Because we do not physically remove a existing child from its parent.

```
class TrieNode:
    def __init__(self):
        self.chilren = {}
        self.isWord = False
        self.hasWord = False
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
    """
    At each level we also updates hasWord to True 
    """
        if word == '':
            self.root.isWord = True
            self.root.hasWord = True
         
        ptr = self.root
        for c in word:
            if c not ptr.children:
                ptr.children[c] = TrieNode()
            ptr.hasWord = True
            ptr = ptr.children[c]
        ptr.isWord = True
        ptr.hasWord = True
        
    def find(self, word)：
        if word == '':
            return self.root.isWord
        
        ptr = self.root
        for c in word:
            #　because the node may contains child that has been remove but still exist in children list, we need to first check whether it is still valid
            if not ptr.hasWord:
                return False
                
            if c in ptr.children:
                ptr = ptr.children[c]
            else:
                return False
        return ptr.isWord

    def findPerfix(self, prefix):
        if prefix == '':
            return self.root.isWord
        
        ptr = self.root
        for c in prefix:
            if not ptr.hasWord:
                return False
                
            if c in ptr.children:
                ptr = ptr.children[c]
            else:
                return False
        return True
        
    def remove(self, word):
        if word == '':
            self.root.isWord = False
            self.root.hasWord = False
            
        def helper(node, key):
            if key == '':
                node.isWord = False
                node.hasWord = False
                return
             
             # the word is not in the trie
             if key[0] not in node.children:
                 return
             helper(node.children[key[0]], key[1:])
             # backtrack and update parent flag with children flags
             node.hasWord = any([child.hasWord for child in node.children.values()])            
```

[Spelling Corrector](http://www.zhihu.com/question/29592463)