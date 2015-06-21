# String

###Longest Palindromic Substring
Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

Example  
Given the string = "abcdzdcab", return "cdzdc".

##### Analysis
1. Brute Force: $$O(n^3)$$  
check each substring ($$O(n^2)$$) which is palindromic $$O(n)$$

2. Dynamic Programming: $$O(n^2)$$ with space: $$O(n^2)$$
