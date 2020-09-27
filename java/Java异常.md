[TOC]

# Java异常

## 异常概述

编译时期异常（Exception）：在编译时期就出现的异常，必须立即处理。

运行时期异常（Runtime）：在程序运行时出现的异常，可以不处理（但不安全）。如指针越界。

错误：error（严重错误）

## 异常处理

处理方式：**try...catch**处理方式

​	一个异常的情况

```java
try {
    可能有异常的语句	//在这里如果出现异常，会创建一个相应的异常对象，然后catch接收这个对象进行处理。
} catch(异常类 异常对象){
	处理语句
}
```

​	多个异常的情况：

​		平级关系

```java
try {
    可能有异常的语句
} catch(异常类1 | 异常类2 | ...  异常对象){
	处理语句
}
```

​		存在上下级关系

```java
try {
    可能有异常的语句
} catch(异常类1 异常对象){
	处理语句
} catch(异常类2 异常对象){
	处理语句
} catch(异常类3 异常对象){
	处理语句
}		//此种异常处理方式，处理异常的范围必须是逐级递增的。
```

### 异常处理常用方法（Throwable中的方法）

#### getMessage()

作用：返回throwable的详细信息字符串

#### toString()

作用：返回此可抛出的简短描述。结果是：

- 这个对象的类的name

- 冒号+空格

- 调用getLocalizedMessage()方法的结果，如果getLocalizedMessage返回null，那么只返回类名。默认返回getMessage()。

#### printStackTrace()

作用：调用toString() + 问题出现的位置	结果输出在控制台上。

主要使用这个方法。

## throws

定义功能方法时，需要把出现的问题暴露出来让调用者去处理。那么就通过throws在方法上表识。

告诉别人，我有问题，给你啦。

### throw概述

如果出现了异常，我们可以把该异常抛出，这个时候的抛出异常应该是异常的对象。

### throw和throws区别

- **throws**
  - 用在方法声明后面，跟的是异常类名
  - 可以跟多个异常类名，用逗号隔开
  - 表示抛出异常，由该方法的调用者来处理
  - throws表示出现异常的一种可能性，并不表示一定会发生这些异常

- **throw**
  - 用在方法体内，跟的是异常对象名
  - 只能抛出一个异常对象
  - 表示抛出异常，由方法体内的语句处理
  - throw则是抛出了异常，执行throw一定是抛出了某种异常

## 我们到底该如何处理异常

- 原则：如果该功能内部可以将问题处理，用try，如果处理不了，交由调用者处理，这是throw

- 区别：后续程序需要继续运行就try，后续程序不需要运行就throws

- 举例：
  - 感冒了就自己吃点药就好了（try）
  - 吃了好几天药都没好，那就得（throw）到医院
  - 如果医院没有特效药就变成了error

## finally的特点作用及其面试题

- finally的特点
  - 被finally控制的语句体一定会执行
  - 特殊情况，在执行finally之前jvm退出了（比如System.exit(0))

- finally的作用
  - 用于释放资源，在IO流操作和数据库操作中会见到

- finally相关面试题
  - final、finally、finalize的区别
    - final：是最终的意思，可以修饰类（类不能继承），成员变量（变量是常量），成员方法（不能重写）；
    - finally：一般来说，代码肯定会执行，特殊情况：在执行finally之前JVM退出了；
    - finalize：是Object类的一个方法，由于释放资源。
  - 如果执行catch里面有return语句，请问finally的代码还会执行吗？如果会，请问是在return前还是return后。
    - 会，前。
    - 准确的说是在中间。

## 自定义异常

如果java没有对应的异常，那么就需要我们自己来定义一个异常。例如考试分数在100分之间。

- 自定义异常
  - 继承Exception（编译时）
  - 继承RuntimeException（运行时）

```java
public class ExceptionDemo extends Exception {
	public ExceptionDemo(){}
	
	public ExceptionDemo(String message) {
		super(message);
	}
}
```

## 异常注意事项

- 子类覆盖父类方法时，子类方法必须抛出相同的异常或父类异常的子类。（父亲坏了，儿子不能比父亲更坏）
- 子类不能抛出父类没有的异常
- 如果被覆盖的方法没有异常抛出，那么子类的方法绝对不可以抛出异常，如果子列方法内有异常发生，那么子类只能throws。

抛出异常

```java
public class ThrowTest {

    public static void main(String[] args) {
        Integer a = 1;
        Integer b = null;
        //当a或者b为null时，抛出异常
        if (a == null || b == null) {
            throw new NullPointerException();
        } else {
            System.out.println(a + b);
        }
    }
}
```

异常匹配

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class MultipleCapturesDemo {
    public static void main(String[] args) {
        try {
            new FileInputStream("");
        } catch (FileNotFoundException e) {
            System.out.println("IO 异常");
        } catch (Exception e) {
            System.out.println("发生异常");
        }
    }
}
```

自定义异常（基础Exception或其子类）

```java
// MyAriException.java
public class MyAriException extends ArithmeticException {
    //自定义异常类，该类继承自ArithmeticException

    public MyAriException() {

    }
    //实现默认的无参构造方法

    public MyAriException(String msg) {
        super(msg);
    }
    //实现可以自定义输出信息的构造方法，将待输出信息作为参数传入即可
}
```

