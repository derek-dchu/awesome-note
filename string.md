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



