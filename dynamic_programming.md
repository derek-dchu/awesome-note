# Dynamic Programming

# Longest Common Substring (LCS)
Suppose we have a source string with length $$p$$ and a target string with length $$q$$, then we can construct a $$\mathit{LCSuffix}$$ table using following formular.

$$
\mathit{LCSuff}(S_{1..p}, T_{1..q}) =
\begin{cases}
       \mathit{LCSuff}(S_{1..p-1}, T_{1..q-1}) + 1  \mathrm{if } \; S[p] = T[q] \\
       0                                            \mathrm{otherwise}.
\end{cases}
$$

The Longest Common Substring can be found using following formular.
$$
\mathit{LCSubstr}(S, T) = \max_{1 \leq i \leq m, 1 \leq j \leq n} \mathit{LCSuff}(S_{1..i}, T_{1..j}) \;
$$

Pseudocode
```

```