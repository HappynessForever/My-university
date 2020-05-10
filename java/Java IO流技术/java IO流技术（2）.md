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

### 文件字节输入流

```java
//接口InputStream
//把刚才写入的数据读取出来显示在控制台
//构造方法
FileInputStream(File file);
FileInputStream(String name);
//成员方法
public int read();			//返回的是字节的阿斯克码值
public int read(byte b[]);	//返回值是实际读取的字节个数
```



```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo {
	public static void main(String[] args) {
		FileInputStream fis = null;
		try {
			fis = new FileInputStream("d:\\IO.txt");
			int by = 0;
			while((by = fis.read()) != -1) {
				System.out.print((char)by);
			}
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(fis != null) {
				fis.close();
			}
		}
	}
}

```

### 文件字节输出流

```java
//接口OutputStream
//构造方法，刷新输出流
FileOutputStream(File name);
FileOutputStream(String file);	
//构造方法，追加输出流
FileOutputStream(File name, boolean append);
FileOutputStream(String file, boolean append);
//使用输出流写字节
void writer(int n);
void writer(byte b[]);
void writer(byte b[], int off, int len);
//关闭流
void close();
```

这两个构造方法实质上是一样的。如果输出流指向的文件不存在，java就会创建该文件，如果指向的文件是已经存在的文件，输出流将刷新该文件。**为什么一定要close？**让对象变成垃圾，这样就可以被垃圾回收器回收，还有就是通知系统去释放跟该文件相关的文件的资源。

字节流写数据常见问题

- 创建字节输出流到底做了哪些事情？
- 数据写成功后，为什么要close()
- 如何实现数据的换行？写入换行符“\n”，不过有的能换行，有的不能换行，但是有的不兼容。windows换行为“\r\n”。
- 如何实现数据的追加写入？对已存在的文件内容进行追加（不删除原来的数据）。构造方法。

### 写入数据异常处理问题

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo {
	public static void main(String[] args) {
		FileOutputStream fos = null;
		try {
			fos = new FileOutputStream("d:\\IO.txt");			
			fos.write("Hello World2".getBytes());			
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(fos != null) {
				//需要判断fos是否为空
				try {
					fos.close();	//保证会关闭系统资源
				} catch (IOException e) {
					e.printStackTrace();
				}
			}			
		}
	}
}

```

### 字节流复制文本案例

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyDemo {
	public static void main(String[] args) throws IOException {
		//封装数据源
		FileInputStream fis = new FileInputStream("a.txt");
		
		//封装目的地
		FileOutputStream fos = new FileOutputStream("b.txt");
		
		int by = 0;
		while((by = fis.read()) != -1) {
			fos.write(by);
		}
		
		//释放资源
		fis.close();
		fos.close();
	}
}
```

注意：这里读取字符没有进行类型转换，故中文不会乱码，但是如果一个字节一个字节进行类型转换，中文就会出现乱码现象。在计算机中中文存储分两个字节：其中一个字节代表负值。

### 字节流复制图片案例

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyImaageDemo {
	public static void main(String[] args) throws IOException {
		//封装数据源
		FileInputStream fis = new FileInputStream("C:\\Users\\吕骏\\Pictures\\Camera Roll\\220491.jpg");
		
		//封装目的地
		FileOutputStream fos = new FileOutputStream("new.jpg");
		
		int by = 0;
		while((by = fis.read()) != -1) {
			fos.write(by);
		}
		
		//释放资源
		fis.close();
		fos.close();
	}
}

```

改进：采用字节数组加快速度

```java
byte[] bys = new byte[1024];	//字节数组的大小为1024或1024的倍数
		int len = 0;
		while((len=fis.read(bys)) != -1) {
			fos.write(bys, 0, len);
		}
```

形象理解：一次喝一滴水和一次喝一口水。

## 缓冲流

- 字节流一次读写一个数组的速度明显比一次读写一个字节的速度快很多，这是加入了数组这样的**缓冲区**效果，Java本身在设计的时候，也考虑到了这样的设计思想，所以提供了相关缓冲流。
- 字节缓冲输出流
  - BufferedOutputStream
- 字节缓冲输入流
  - BufferedInputStream
- 字符缓冲输入流
  - BufferedReader
- 字符缓冲输出流
  - BufferedWriter

方法分析：

BufferedOutputStream(OutputStream out)；为什么不传递一个具体的文件或者文件路径，而是传递一个OutputStream对象？字节缓冲区流仅仅是提供一个缓冲区，为高效而设计的，而真正的读写操作还得基本的流对象来实现。

```java
BufferedOutputStream bos = new BufferedOutputStream(new 		  	FileOutputStream("a.txt"));	
```

**四种方式进行复制操作**

- 基本字节流一次读写一个字节（117235毫秒）
- 基本字节流一次读写一个字节数组（156毫秒）
- 高效字节流一次读写一个字节（1141毫秒）
- 高效字节流一次读写一个字节数组（47毫秒）这或许就是就是限速的原因吧

测试时间方法：

```java
long start = System.currentTimeMillis();

long end = System.currentTimeMillis();
```

# 总结

（1）IO用于在设备间进行数据传输的操作

（2）分类：

- 流向
  - 输入流 读取数据
  - 输出流 写入数据
- 数据类型
  - 字节流
    - 字节输入流
    - 字节输出流
  - 字符流
    - 字符输入流
    - 字符输出流

（3）FileOutputStream写入数据

操作步骤：创建字节输出流对象，调用write()方法，释放资源。

```java
FileOutputStream fos = new FileOutputStream("a.txt");
fos.write("hello world".getBytes());
fos.close();
```

要注意的问题：

- 创建字节输出流对象做了几件事情？
- 为什么要close()？
- 如何实现数据的换行？
- 如何实现数据的追加写入？

（4）FileInputStream读取数据

操作步骤：创建字节输入流对象，调用read()方法，释放资源。

```java
FileInputStream fis = new FileInputStream("a.txt");
//方式一
int by = 0;
while((by=fis.read()) != -1){
    System.out.print((char)by);
}
//方式二
byte[] bys = new byte[1024];
int len = 0;
while((len=fis.read(bys, 0, len)) != -1){
    System.out.print(new String(bys, 0, len));
}
fis.close();
//注：两种方式只能选择其中一个。
```

（5）案例：

- 复制文本文件
- 复制图片
- 复制视频

（6）缓冲流

字节缓冲区流、字符缓冲区流。

（7）分类

- 字节流
  - InputStream
    - FileInputStream
    - BufferedInputStream
  - OutputStream
    - FileOutputStream
    - BufferedOutputStream
- 字符流
  - Reader
    - FileReader
    - BufferedReader
  - Writer
    - FileWriter
    - BufferedWriter