[TOC]

## IO流概述

- IO流用来处理设备之间的数据（上传文件和下载文件）
- Java对数据的操作是通过流的方式
- Java用于操作流的对象都在IO包中

## IO流分类

- 按照数据流向（从程序的角度来看，文件上的内容输入到程序，故是输入流，反之是输出流）
  - 输入流 读入数据（文件输入到程序）InputStream、Reader
  - 输出流 写入数据（程序输出到文件）OutputStream、Writer

- 按照数据类型
  - 字节流
  - 字符流
    - 该如何选择？如果数据所在文件通过windows自带的记事本打开能读懂里面的内容，就用字符流，如果你什么都不知道，就用字节流。

## IO流常用类

- 字节流的抽象基类：InputStream，OutputStream
- 字符流的抽象基类：Reader，Writer

注：由这四个类派生出来的自来名称都是以其父类作为子类名的后缀。如FileInputStream、FileReader。

