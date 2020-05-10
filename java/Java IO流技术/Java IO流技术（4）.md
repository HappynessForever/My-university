[TOC]

## 三种方式实现键盘输入

```java
//main方法的args接收参数
```

```java
Scanner sc = new Scanner(System.in);
```

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
br.readLine();	//在Scanner出现之前使用的方式
```

**输出**

```java
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
```

转换流的应用：字节流转换为字符流，从而进行字符流的操作。

## 随机流

​		RandomAccessFile类不属于流，是**Object类**的子类。但它融合了InputStream和OutputStream的功能。支持对随机访问文件的读取和写入。

```java
//构造方法
//第一个参数是文件路径，第二个参数是操作文件的模式（常用rw）
RandomAccessFile(String name, String mode);
RandomAccessFile(File file, String mode);

//常用方法
getFilePointer();	//获取当前读写位置
seek(long position);//定位读写位置
```

## 序列化流

- 序列化流
  - ObjectOutputStream 把**对象**按照流一样的方式存入文本文件或者在网络中传输。对象——流数据（写）
- 反序列化流
  - ObjectInputStream 把文本文件中的流对象数据或者网络中的流对象数据还原成对象。流数据——对象（读）
- 序列化操作问题
  - 为什么要实现序列化？
  - 如何实现序列化？
  - 序列化数据后，再次修改类文件，读取数据会出现问题，如何解决？
  - 使用**transient**关键字声明不需要序列化的成员变量。

创建一个对象，并且实现一个标记接口——**Serializable**

```java
package 序列化流;

import java.io.Serializable;

public class Person implements Serializable{
	private String name;
	private transient int age;	//使age不被序列化
	public Person() {
		super();
	}
    private static final long serialVersionUID = -8036188228921520309L;//给定一个id，使id匹配问题得以解决。以后对类进行任何改动，它读取以前的数据是没有问题的。
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
	
}

```

测试

```java
package 序列化流;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class ObjectStreamDemo {
	public static void main(String[] args) throws IOException, ClassNotFoundException {
		
		write();
		
		read();
	}

	private static void read() throws IOException, IOException, ClassNotFoundException {
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("oos.txt"));
		
		Object obj = ois.readObject();
		
		ois.close();
		
		System.out.println(obj);
	}

	private static void write() throws IOException {
		//创建序列化流对象
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("oos.txt"));
		
		//创建对象
		Person p = new Person("吕骏", 20);
		
		//写入序列化对象
		oos.writeObject(p);
		
		//释放资源
		oos.close();
		
		
	}
}

```

## Properties

属性集合类，是一个可以和IO流结合使用的集合类。可以保存在流中或从流中加载，属性列表中每个键及其值都是一个字符串。

```java
package propersites;

import java.util.Properties;
import java.util.Set;

public class PropertiesDemo {
	public static void main(String[] args) {
		Properties prop = new Properties();
		
		//添加元素
		prop.put("1", "A");
		prop.put("2", "B");
		prop.put("3", "C");
		prop.put("4", "D");
		prop.put("5", "E");
		
		//遍历集合
		Set<Object> set = prop.keySet();
		for(Object key : set) {
			Object value = prop.get(key);
			System.out.println(key + "---" + value);
		}
	}
}

```

**特殊用法**

```java
public Object setProperty(String key, String value);//添加元素
public String getProperty(String key);//获取元素
public Set<String> stringPropertyName();//获取所有的键的集合
```

**Properties和IO流结合使用**

```java
public void load(Reader reader);	//把文件的内容存储到集合中
public void store(Writer writer, String comments);//把集合中的数据存储到文件中，第二个参数数属性列表的描述。
```

注意：文件的数据必须是键值对形式。 

单机版游戏：进度保存和加载。

判断文件是否有指定的键，若果有就修改值的案例

```java
package propersites;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.Reader;
import java.io.Writer;
import java.util.Properties;
import java.util.Set;

public class PropertiesDemo {
	public static void main(String[] args) throws IOException {
		Properties prop = new Properties();
		
		Reader r = new FileReader("a.txt");	//加载文件存储到集合中
		prop.load(r);
		r.close();
		
		//遍历集合
		Set<String> set = prop.stringPropertyNames();
		for(String key : set) {
			if("吕骏".equals(key)) {
				prop.setProperty(key, "100");
				break;
			}
		}
		
		Writer w = new FileWriter("a.txt");
		prop.store(w, "修改吕骏值的信息");
		w.close();
	}
}
```

## NIO包下的IO流

新IO使用了不同的方式来处理输入输出，采用内存映射文件的方式，将文件或者文件的一段区域映射到内存中，就可以像访问内存一样来访问文件了，效率比旧IO要高。但目前主流还是旧IO流。