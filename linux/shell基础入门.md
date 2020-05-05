[TOC]

## Shell基础入门

shell脚本（shell script），是一种为shell编写的脚本程序；

适合对服务器管理；

### 第一个Shell脚本

新建一个a.sh文件，要求输出Hello World

```
#!/bin/bash
echo "Hello World"
```

#### 运行Shell脚本有两种方法：

**1、作为可执行程序**

首先需要设置可执行权限

```
# chmod +x a.sh
```

执行脚本

```
# ./a.sh
```

注意：一定要写成	./a.sh	，而不是a.sh。

2、作为解释器参数

```
# /bin/sh a.sh	或	# sh a.sh
```

### Shell变量

#### 定义变量

​		数字、字母、下划线，且数字不能开头。

#### 使用变量

​		只要在变量名前面加美元符号就好。推荐给所有变量加上花括号，这是个好的编程习惯。

```
#!/bin/bash
my_name="吕骏"
echo $my_name
echo ${my_name}
```

#### 只读变量

​		使用 **readonly** 命令，只读变量不允许被修改。

```
#/bin/bish
my_name="吕骏"
readonly my_name
```

#### 删除变量

​		使用 **unset** 命令。

```
unset my_name
```

读入数据

使用 **read** 命令，读取来自键盘输入的值并存入变量

```
read [-pt] 变量名
```

参数说明：

-p	后面可以接提示信息

-t	后面接等待的秒数，为了防止一致等待用户输入

#### 变量类型

​		1）**局部变量**	局部变量在脚本或命令中定义，仅在当前shell实例中有效。

​		2）**环境变量**	所有程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。

​		3）**shell变量**	shell变量是shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

常见的几个shell变量（预定义变量）

$#	存储shellcx中命令行参数个数

$?	存储shell中上一个程序执行的返回值（0表示命令执行成功，非0则表示程序执行有问题）

$[1-n]	存储第[1-n]个命令行参数

$0	存储shell程序自己的名称

### Shell test命令

#### 数值测试

| 参数 | 说明           |
| :--- | :------------- |
| -eq  | 等于则为真     |
| -ne  | 不等于则为真   |
| -gt  | 大于则为真     |
| -ge  | 大于等于则为真 |
| -lt  | 小于则为真     |
| -le  | 小于等于则为真 |

#### 字符串测试

| 参数      | 说明                     |
| :-------- | :----------------------- |
| =         | 等于则为真               |
| !=        | 不相等则为真             |
| -z 字符串 | 字符串的长度为零则为真   |
| -n 字符串 | 字符串的长度不为零则为真 |

#### 文件测试

| 参数      | 说明                                 |
| :-------- | :----------------------------------- |
| -e 文件名 | 如果文件存在则为真                   |
| -r 文件名 | 如果文件存在且可读则为真             |
| -w 文件名 | 如果文件存在且可写则为真             |
| -x 文件名 | 如果文件存在且可执行则为真           |
| -s 文件名 | 如果文件存在且至少有一个字符则为真   |
| -d 文件名 | 如果文件存在且为目录则为真           |
| -f 文件名 | 如果文件存在且为普通文件则为真       |
| -c 文件名 | 如果文件存在且为字符型特殊文件则为真 |
| -b 文件名 | 如果文件存在且为块特殊文件则为真     |

#### 条件判断符

1）能实现和text命令一样的功能

2）为了与通配符区分，各元素间均有空格

### 流程控制

#### 条件判断（if-else）

```
if condition
then
	command1
	command2
	...
	commandN
fi
或
if condition
then
	command1
	command2
	...
	commandN
else
	command
fi
或
if condition
then
	command1
elif condition2
then
	command2
else
	command3
fi	
```

#### case 

Shell case语句为多选择语句。可以用case语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。格式如下：

```
case 值 in
模式1)
	commamd;;
模式2)
	command;;
esac
```

#### for 循环

语法一：

```
for((变量赋值; 条件判断; 变量迭代))
do
	语句块
done
```

语法二：

```
for var in item1 item2 ... itemN
do 
	command1
	command2
	...
	commandN
done
```

#### while 语句

while循环用于不断执行一系列命令，也用于从输入文件中读取数据，格式如下：

```
while condition
do
	command
done
```

