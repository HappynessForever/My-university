# JDK5新特性

- 自动装箱和拆箱
- 泛型
- 增强for循环
- 静态导入
- 可变参数
- 枚举

## 枚举

- 枚举概述
  - 是指将变量的值一一列出来，变量的值只限于列举出来的值的范围内。举例：一周有七天，一年只有12个月等。
  - 回想单例设计模式：单例类是一个类只有一个实例
  - 那么多例类就是一个类有多个实例，但不是无限个数的实例，而是有限个数的实例。这才能是枚举类。

```java
package 枚举;

public abstract class Direction {
	public static final Direction FRONT = new Direction("前") {

		@Override
		public void show() {
			System.out.println("前");
			
		}
		
	};
	public static final Direction EEHIND = new Direction("后"){

		@Override
		public void show() {
			System.out.println("后");
			
		}
		
	};
	public static final Direction LEFT = new Direction("左"){

		@Override
		public void show() {
			System.out.println("前");
			
		}
		
	};
	public static final Direction RIGHT = new Direction("右"){

		@Override
		public void show() {
			System.out.println("右");
			
		}
		
	};
	
	private String name;
	private Direction(String name) {
		this.name = name;
	}
	
	public String getName() {
		return name;
	}
	
	public abstract void show();
}

```

## 通过enum实现枚举类

```java
package cn02;

public enum Direction {
	FRONT("前") {
		@Override
		public void show() {
			System.out.println("前");
			
		}
	},EEHIND("后") {
		@Override
		public void show() {
			System.out.println("后");
			
		}
	},LEFT("左") {
		@Override
		public void show() {
			System.out.println("左");
			
		}
	},RIGHT("右") {
		@Override
		public void show() {
			System.out.println("右");
			
		}
	};
	
	private String name;
	private Direction(String name) {
		this.name = name;
	}
	
	public String getName() {
		return name;
	}
	
	public abstract void show();
}

```

## 枚举类中的几个常用方法

- int compareTo（E o）
- String name（）
- int ordinal（）
- <T> T valueOf(Class<T> type, String name)
- values()



输出：Hello World

```java
import java.util.Scanner;

public class Hello {
	public static void main(String[] args) {
		System.out.println("Hello World");
		changliang();
		zifuchuan("apples");
		sum();
	}
	
	static void changliang() {
		final double PI = 3.14;
		System.out.println(PI);
	}
	
	static void zifuchuan(String str) {
		System.out.println("字符串长度为：" + str.length());
		System.out.println(str.equals("apples"));
		System.out.println(str.concat("lvjun"));
		System.out.println(str.charAt(0));
		System.out.println(str.indexOf("apples"));
		System.out.println(str.substring(1));
	}
	
	static void sum() {
		Scanner in = new Scanner(System.in);
		
		int a = in.nextInt();
		int b = in.nextInt();
		
		in.close();
		System.out.println("a+b=" + (a + b));
	}
}

```

