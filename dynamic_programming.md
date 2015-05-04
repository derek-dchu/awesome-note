# Dynamic Programming

# Longest Common Substring (LCS)
Suppose we have a source string with length $$p$$ and a target string with length $$q$$, then we can construct a $$\mathit{LCSuffix}$$ table using following formular.

$$
\mathit{LCSuff}(S_{1..p}, T_{1..q}) = (S[p] = T[q]) \, ? \, \mathit{LCSuff}(S_{1..p-1}, T_{1..q-1}) + 1 : 0
$$

The Longest Common Substring can be found using following formular.
$$
\mathit{LCSubstr}(S, T) = \max_{1 \leq i \leq m, 1 \leq j \leq n} \mathit{LCSuff}(S_{1..i}, T_{1..j}) \;
$$

Pseudocode
```
function LCSubstr(S[1..m], T[1..n])
    LCSuffix := matrix(m, n)
    max := 0
    result := {}
    for i := 1..m
        for j := 1..n
            if S[i] == T[j]
                if i == 1 or j == 1
                    LCSuffix[i,j] := 1
                else
                    LCSuffix[i,j] := LCSuffix[i-1,j-1] + 1
                if LCSuffix[i,j] > max
                    max := LCSuffix[i,j]
                    result := {S[i-max+1..i]}
                else
                if LCSuffix[i,j] == max
                    result := result ∪ {S[i-z+1..i]}
            else 
                LCSuffix[i,j] := 0
    return result
```