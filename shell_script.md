# Shell Script

## Commands

### List the nth line of a file
1.  `head -n${n} file | tail -n1`
2.  `sed '${n}p;d' file`
3.  `awk 'NR==${n}{print;exit}' file`