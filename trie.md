# Trie

### Make trie
```python
END = '$'

def make_trie(words):
    trie = {}
    for word in words:
        t = trie
        for c in word:
            if c not in t: t[c] = {}
            t = t[c]
        t[END] = {}
    return trie
```

[Spelling Corrector](http://www.zhihu.com/question/29592463)