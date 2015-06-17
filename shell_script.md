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

### Validate Phone Numbers
[Description](https://leetcode.com/problems/valid-phone-numbers/)

```
grep -P '^(\(\d{3}\)\s|\d{3}-)\d{3}-\d{4}$' file.txt
```

### Execute Command Without Output
Redirect output(`stdout`) to `/dev/null`.

If we also want to ignore err, we need to redirect `stderr` as well by using `2>&1`.

Eventually, we have
```
$ <command> > /dev/null 2>&1
```


