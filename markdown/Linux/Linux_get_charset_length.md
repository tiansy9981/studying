# Linux获取字符串长度的方法：



求字符串操作在shell脚本中很常用，下面归纳、汇总了求字符串的几种可能方法:

【方法一】:利用${#str}来获取字符串的长度

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373"
[vpn.harbor root ~] $ echo ${#str}
40
```



【方法二】:利用awk的length方法

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373"
[vpn.harbor root ~] $ echo ${str} | awk '{print length($0)}'
40
```

> 备注:
>
> 1. 最好用{}来放置变量
> 2. 也可以用length($0)来统计文件中每行的长度

```bash
[vpn.harbor root ~] $ awk '{print length($0)}' /etc/passwd
31
32
39
36
40
31
44
32
46
```



【方法三】:利用awk的NF项来获取字符串长度

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373"
[vpn.harbor root ~] $ echo $str | awk -F "" '{print NF }'
40
```

> 备注: -F为分隔符，NF为域的个数，即单行字符串的长度

 

【方法四】:利用wc的-L参数来获取字符串的长度

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373"
[vpn.harbor root ~] $ echo $str | wc -L
40
[vpn.harbor root ~] $ wc -L /etc/passwd
99 /etc/passwd
```

> 备注: -L参数
>
> 1. 对多行文件来说，表示打印最长行的长度！ 99，表示/etc/passwd文件最长行的长度为99
> 2. 对单行字符串而言，表示当前行字符串的长度！



【方法五】:利用wc的-c参数，结合echo -n参数

```bash
[vpn.harbor root ~] $ echo -n  "24b9da6552252987aa493b52f8696cd6d3b00373" |wc -c
40
[vpn.harbor root ~] $ echo   "24b9da6552252987aa493b52f8696cd6d3b00373" |wc -c
41
```

> 备注: 
>
> 1. -c参数: 统计字符的个数
> 2. -n参数: 去除"\n"换行符，不去除的话，默认带换行符，字符个数就成了41



【方法六】:利用expr的length方法

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373"
[vpn.harbor root ~] $ expr length $str
40
```



【方法七】:利用expr的$str : ".*"技巧

```bash
[vpn.harbor root ~] $ str="24b9da6552252987aa493b52f8696cd6d3b00373";  expr $str : ".*"
40
```

> 备注: .*代表任意字符，即用任意字符来匹配字符串，结果是匹配到40个，即字符串的长度为40