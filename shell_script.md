# Shell Script

## Commands

### List the nth line of a file
```shell
1.  head -n ${n} file | tail -n 1
2.  sed '${n}p;d' file
3.  sed -n '${n}p' file
4.  awk 'NR==${n}{print;exit}' file
```

### List from line m to line n
```
1.  head -n ${n} file | tail -n ${$($n-$m)}
2.  sed '${m},${n}p;d' file
3.  sed -n '${m},${n}p' file
4.  awk 'NR>=${m}&&NR<=${n}{print;exit}' file
```

### Word Frequency
[Description](https://leetcode.com/problems/word-frequency/)

```
cat words.txt | tr -s ' ' '\n' | sort | uniq -c | sort -n -r | awk '{print $2" "$1}'
```