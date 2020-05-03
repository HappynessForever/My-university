[TOC]

## Git基本操作命令

### git init

用git init在目录中创建行的Git仓库。示例：

随便找一个或创建一个目录，在这个目录中使用git init命令。

```
$ git init
Initialized empty Git repository in /Users/tianqixin/www/runoob/.git/
```

### git clone

使用 git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。如果你需要与他人合作一个项目，或者想要复制一个项目，看看代码，你就可以克隆那个项目。 执行命令：

```
 git clone [url]
```

### git add

git add 命令可将该文件添加到缓存。如我可以添加2个文件：

```
$ touch a.txt
$ touch b.txt
$ ls
a.txt	b.txt
$ git add a.txt b.txt
```

git add .添加当前项目的所有文件

```
$ git add .
```

### git status

git status 以查看在你上次提交之后是否有修改。常用参数 -s ，获得简短的结果输出。

### git commit

使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。常用参数 -m 添加提交注释信息。

```
$ git commit -m "添加一个文件"
```

### git rm

如果只是简单地从工作目录中手工删除文件，运行 **git status** 时就会在 **Changes not staged for commit** 的提示。

```
$ git rm <file>
```

### git mv

git mv 命令用于移动或重命名一个文件、目录、软连接。例如下面代码是将a.txt文件名改为b.txt。

```
$ git mv a.txt b.txt
```

