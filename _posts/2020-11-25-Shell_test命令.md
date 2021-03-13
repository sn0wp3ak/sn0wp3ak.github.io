---
layout: post
title: Shell_test命令
date: 2020-11-25
categories:
- shell
---

Shell脚本中test命令用于检查条件是否成立, 可以进行数值, 字符, 文件3个方面的测试<br>

### 数值测试

> -eq 等于<br>
> -ne 不等于<br>
> -gt 大于<br>
> -lt 小于<br>
> -ge 大于等于<br>
> -le 小于等于<br>


```shell
num1 = 100
num2 = 200
if test $[num1] -eq $[num2]
then
    echo '两个数相等'
else
    echo '两个数不相等'
fi
```
```
两个数不相等
```
方括号`[]`中的表达式执行基本的算数运算<br>
```shell
#!/bin/bash
a=10
b=20

res=$[a+b]
echo "res的值为: $res"
```
```
res的值为: 30
```

### 字符串测试

> =  等于<br>
> != 不等于<br>
> -z 长度是否为0<br>
> -n 长度是否不为0<br>

```shell
str1="hello"
str2="aloha"
if test $str1 = $str2
then
    echo "2个字符串相等"
else
    echo "2个字符串不相等"
fi
```
```
2个字符串不相等
```

### 文件测试

> -e file  是否存在<br>
> -r file  是否存在且可读<br>
> -w file  是否存在且可写<br>
> -x file  是否存在且可执行<br>
> -s file  是否为非空文件<br>
> -d file  是否存在且为目录<br>
> -f file  是否存在且为普通文件<br>
> -c file  是否存在且为字符设备文件<br>
> -b file  是否存在且为块设备文件<br>


```shell
cd /bin
if test -e ./bash
then 
    echo '文件存在'
else
    echo '文件不存在'
fi
```
```
文件存在
```

### 连接测试条件

> -a  与<br>
> -o  或<br>
> !   非<br>

优先级: `!`最高, `-a`次之, `-o`最低<br>

```shell
cd /bin
if test -e ./zsh -o -e ./bash
then
    echo '至少存在1个文件'
else
    echo '都不存在'
fi
```
```
至少存在1个文件
```




