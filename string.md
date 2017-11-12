# String

## Pattern Matching

Find the start position of substrings in Text which matches given Patterns.

#### 1. Brute Force

O\(\|Text\| \* \|Patterns\|\)

#### 2. Pattern Prefix Trie Matching

O\(\|Text\| \* \|LongestPattern\|\), here we encode patterns into a prefix trie.

It takes O\(\|Patterns\|\) to construct a Trie.

##### Issue

Memory Footprint is High! Number of edges in a Trie = O\(\|Patterns\|\)

### 3. Text Suffix Trie Matching

O\(\|Patterns\|\), here we encode patterns into a suffix trie.

It takes O\(\|Text\|\) to construct a suffix trie.

##### Issue

Memory Footprint is High! Number of edges in a Trie = O\(\|Text\|^2\)

### 4. Text Suffix Tree Matching

O\(\|Text\| + \|Patterns\|\), here we encode patterns into a suffix tree.

It takes O\(\|Text\|\) to construct a suffix tree trie.

Memory footprint of a suffix tree trie is O\(\|Text\|\) by saving leaf branch as the \(start position, end position\) of the leaf branch.

### 5. Burrow-Wheeler Transform

BWT\(S\) is the last column of sorted cyclic rotation of S.

```
GAGA$ -> GAGA$ -> $GAGA -> AGGA$
         $GAGA    A$GAG
         A$GAG    AGA$G
         GA$GA    GA$GA
         AGA$G    GAGA$
```

Time: O\(\|Text\|\)

##### First-Last Property

* the k-th occurrence of symbol in FirstColumn and k-th occurrence of symbol in LastColumn correspond to appearance of symbol at the same position of Text.

#### Inverting BWT

Start with $ in FirstColumn, jump between FirstColumn and LastColumn using First-Last Property.

Time: O\(\|Text\|\), Memory: 2\|Text\|.

#### Pattern Matching

```
BWMatching(FirstOccurrence, LastColumn, Pattern, Count):
    """
    FirstOccurrence: a array contains first occurrences of symbol in firstColumn
    Count: matrix of size(n_symbol, len_of_bwt + 1), count[s][i] is # occurrences of symbol in bwt[:i]
    """

    top ← 0
    bottom ← |LastColumn| − 1
    while top ≤ bottom:
        if Pattern is nonempty:
            symbol ← last letter in Pattern
            remove last letter from Pattern
            if positions from top to bottom in LastColumn contain an occurrence of symbol:
                top ← FirstOccurrence(symbol) + Countsymbol(top, LastColumn)
                bottom ← FirstOccurrence(symbol) + Countsymbol(bottom + 1, LastColumn) − 1
            else:
                return 0
        else:
            return bottom − top + 1
```

Time: O\(\|Pattern\|\)

### 6. Suffix Array

Suffix array holds starting position of each suffix beginning a row.

### Longest Palindromic Substring

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

Example  
Given the string = "abcdzdcab", return "cdzdc".

##### Analysis

* Brute Force: $$O(n^3)$$

  Check each substring \($$O(n^2)$$\) whether is palindromic $$O(n)$$.

* Dynamic Programming: $$O(n^2)$$ with extra space: $$O(n^2)$$

  Define `P[ i, j ] ← true` iff the substring $$S_{i ... j}$$ is a palindrome, otherwise false.

  Therefore, `P[ i, j ] ← ( P[ i+1, j-1 ] and Si = Sj )`

  The base cases are:

  `P[ i, i ] ← true`  
    `P[ i, i+1 ] ← ( Si = Si+1 )`

* Expand Around Center: $$O(n^2)$$ with no extra space

  We know that a palindromic mirrors around its center. Therefore we can check a palindromic by expanding from its center. For a string, we can have $$2N-1$$ centers \(N single char, N-1 double chars\). For each center, checking time is $$O(n)$$, eventually, we have $$O(n^2)$$ run-time complexity without extra space.

* [Manacher’s algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring): $$O(n)$$ with extra space: $$O(n)$$



