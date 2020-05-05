[TOC]

### 输出Hello World

```
#!/bin/bash
echo "Hello World"
```

### 判断两个数是否相等

```
#!/bin/bash
a=10
b=20
echo "a=${a},b=${b}"
if test $a -eq $b
then
	echo "a == b"
elif test $a -gt $b
then 
	echo "a > b"
else
	echo "a < b"
fi
```

### 选数字

```
#!/bin/bash
echo "请输入1到4之间的数字："
read num
case $num in
1) echo "你选择了 1" 
;;
2) echo "你选择了 2" 
;;
3) echo "你选择了 3" 
;;
4) echo "你选择了 4" 
;;
5) echo "你输入了其他东西" 
;;
```

### 输出1到3

```
#!/bin/bash
for ((i=1; i<=3; i++))
do
	echo "number is ${i}"
done
```

### 输出一些动物名词

```
#!/bin/bash
for animal in cat dog lion tiger
do 
	echo "The animal is ${animal}"
done
```

### 测试while语句

```
#!/bin/bash
while true
do
	echo "这是一个while无限循环，还好使用break跳出来了。"
	break;
done
```

