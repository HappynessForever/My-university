[TOC]

## 转换流

​		由于字节流操作中文不是特别方便，所以Java就提供了转换流。字符流 = 字节流+编码表。

##  编码表

- 编码表
  - 有字符及其对应的数值组成的一张表
- 常见编码表
  - ASCLL/Unicode字符集
  - ISO-8859-1：拉丁码表 8位表示一个数据
  - GB2312/GBK/GB18030：简体中文
  - BIG5 通用于台湾、香港地区的一个繁体字编码方案，俗称“大五码”
  - UTF-8 最多用三个字节来表示一个字符
  - Unicode 国际标准码（统一各国标准，实现国际化）

## 转换流的使用

```java
OutputStreamWriter(OutputStream out);	//根据默认编码转换为字符流
OutputStreamWriter(OutputStream out, String charsetName);//自己设定编码

//使用方法
OutputStreamWriter osw = new OutputStreamWriter(
                new FileOutputStream("a.txt"));
InputStreamReader isr = new InputStreamReader(
				new FileInputStream("a.txt"));
```

### 五种写数据方式

```java
public void write(int c);
public void write(char[] cbuf);
public void write(char[] cbuf, int off, int len);
public void write(String str);
public void write(String str, int off, int len);
```

### 两种读数据方式

```java
int read();
int read(char[] cbuf);
```

### 字符流复制文本案例

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class FileCopyDemo {
	public static void main(String[] args) throws IOException {
		OutputStreamWriter osw = new OutputStreamWriter(
                new FileOutputStream("b.txt"));
		
		InputStreamReader isr = new InputStreamReader(
				new FileInputStream("a.txt"));
		
		
		char[] chs = new char[1024];
		int len = 0;
		while((len=isr.read(chs)) != -1) {
			osw.write(chs, 0, len);
			osw.flush();	//别忘了需要flush
		} 
		
		isr.close();
		osw.close();
	}
}

```

#### 一般使用其子类

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileCopyDemo {
	public static void main(String[] args) throws IOException {	
		FileReader fr = new FileReader("a.txt");
		
		FileWriter fw = new FileWriter("b.txt");
		
		char[] chs = new char[1024];
		int len = 0; 
		while((len=fr.read(chs)) != -1) {
			fw.write(chs, 0, len);
			fw.flush();
		}
		
		fr.close();
		fw.close();
	}
}
```

## 缓冲流

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Demo {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader("a.txt"));
		
		BufferedWriter bw = new BufferedWriter(new FileWriter("b.txt"));
		
		char[] chs = new char[1024];
		int len = 0;
		while((len=br.read(chs)) != -1) {
			bw.write(chs, 0, len);
			bw.flush();
		}
		
		br.close();
		bw.close();
	}
}

```

### 字符缓冲流的特殊用法

BufferedWriter：public void newLine()；根据系统来决定换行符，就不要考虑换行符不兼容问题。

BufferedReader：public String readLine()；一次读取一行数据，不换行。

**复制文本案例**：一次复制一行。

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Demo {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader("a.txt"));
		
		BufferedWriter bw = new BufferedWriter(new FileWriter("b.txt"));
		
		String line = null;
        while((line=br.readLine()) != null){
            bw.write(line);
            bw.newLine();
            bw.flush();
        }
		
		br.close();
		bw.close();
	}
}

```

# 总结

## 字节流

输出流

- InputStream
  - int read();
  - int read(byte[] bys);
  - FileInputStream
  - BufferedInputStream

输入流

- OutputStream
  - void write(int by);
  - void write(byte[] bys, int off, int len);
  - FileOutputStream
  - BufferedOutputStream

## 转换流

- InputStreamRead(InputStream in);
- OutputStreamWriter(OutputStream out);

## 字符流

字符流 = 字节流 + 编码表

输入流

- Reader
  - int read();
  - int read(char[] chs);
  - InputStreamReader
    - FileReader
  - BufferedReader
    - String readLine()

输出流

- Writer
  - void write(int ch);
  - void write(char[] ch, int offset, int len);
  - OutputStreamWriter
    - FileWriter
  - BufferWriter
    - void newLine();
    - void writer(String str);

字节流复制数据：4种方式

字符流复制数据：5种方式

