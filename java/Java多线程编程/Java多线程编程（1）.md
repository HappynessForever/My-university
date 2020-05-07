[TOC]

# Java多线程编程（1）

​		Java给多线程编程提供了内置的支持。一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。多线程是多任务的一种特别形式，但多线程使用了更小的资源开销。

## 一个线程的生命周期

![YZ3peU.jpg](https://s1.ax1x.com/2020/05/07/YZ3peU.jpg)

- **新建状态**：

  使用new关键字和Thread类或子类建立一个线程对象后，该线程对象就处于新建状态。他保持这个状态直到程序start()这个线程。

- **就绪状态**:

  当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。

- **运行状态:**

  如果就绪状态的线程获取 CPU 资源，就可以执行 **run()**，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。

- **阻塞状态:**

  如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

  - 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
  - 同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。
  - 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。

- **死亡状态:**

  一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。

## 线程的优先级

​		线程默认的优先级为5（范围为1-10），最好使用Java内置的优先级：**Thread.MIN_PRIORITY**、**Thread.MAX_PRIORITY**、**Thread.NORM_PRIORITY**。设置线程的优先级，可以通过 **setPriority（int grade）**来设置。

## 创建一个线程

​		Java 提供了三种创建线程的方法：

- 通过实现 Runnable 接口；
- 通过继承 Thread 类本身；
- 通过 Callable 和 Future 创建线程。

**创建线程的三种方式的对比**：

- 采用实现 Runnable、Callable 接口的方式创建多线程时，线程类只是实现了 Runnable 接口或 Callable 接口，还可以**继承其他类**。

- 使用继承 Thread 类的方式创建多线程时，编写简单，如果需要访问当前线程，则无需使用 Thread.currentThread() 方法，**直接使用 this 即可获得当前线程**。

## 目标对象与线程的关系

从对象和对象之间的关系角度上看，目标对象和线程的关系有以下两种关系。

①	目标对象和线程完全解耦

```java
public class House implements Runnable {
    int waterAmount;	//模拟水量
    public void setWater(int w) {
        waterAmount = w;
    }
    
    public void run() {
        while(true) {
            String name = Thread.currentThread().getName();
            if(name.equals("狗")) {
            	System.out.println(name + "喝水");
                waterAmount -= 2;
            }
            else if(name.equals("猫")) {
            	System.out..println(name + "喝水");
                waterAmount -= 2;
            }
            System.out.println("剩余" + waterAmount);
            try {
                Thread.sleep(2000);
            } catch(InterruptedException e) {
                if(waterAmount <= 0) {
                    return;
                }
            }
        }
    }
}
```

```java
public class ThreadDemo {
    public static void main(String[] args){
        House house = new House();
        house.setWater(10);
        Thread dog, cat;
        //dog和cat的对象相同
        dog = new Thread(house);
        cat = new Thread(house);
        dog.setName("狗");
        cat.setName("猫");
        dog.start();
        cat.start();
    }
}
```

②	目标对象组合线程（弱耦合）

```java
public class House implements Runnable {
    int waterAmount;	//模拟水量
    Thread dog, cat;	//线程是目标对象的成员
    public House(){
        dog = new Thread(this);
        cat = new Thread(this);
    }
    public void setWater(int w) {
        waterAmount = w;
    }
    
    public void run() {
        while(true) {
            Thread t = Thread.currentThread();
            if(t == dog) {
            	System.out.println(name + "喝水");
                waterAmount -= 2;
            }
            else if(t == cat) {
            	System.out..println(name + "喝水");
                waterAmount -= 2;
            }
            System.out.println("剩余" + waterAmount);
            try {
                Thread.sleep(2000);
            } catch(InterruptedException e) {
                if(waterAmount <= 0) {
                    return;
                }
            }
        }
    }
}
```

```java
public class ThreadDemo {
    public static void main(String[] args){
        House house = new House();
        house.setWater(10);     
        house.dog.start();
        house.cat.start();
    }
}
```

注：在实际问题中，应该根据实际情况确定目标对象和线程是组合关系或完全解耦关系，两种关系各有优缺点。

### 关于run方法启动的次数

​		在上述例子中，dog和cat是具有相同目标对象的两个线程，当其中一个线程享有CPU资源时，目标对象自动调用接口中的run方法，当轮到另一个线程享有CPU资源时，目标对象会再次调用接口中的run方法，也就是说run（）方法已经启动运行了两次，分别运行在不同的线程中，即运行在不同的时间片内。

## 线程的常用方法

### start()

​		线程调用该方法将启动线程，使之从新建状态进入就绪队列排队。线程调用start()方法之后，就不必再让线程调用start()方法，否则将导致IllegalThreadStateException异常。

### run() 

​		定义线程对象被调度之后所执行的操作。当run方法执行完毕，线程就变成死亡状态。

### sleep(int millsecond)

​		有时，优先级别高的线程需要优先级别低的线程应该让出CPU资源，使优先级别低的线程有机会执行。sleep方法来使自己放弃CPU资源，休眠一段时间。休眠时间的长短由sleep方法的参数决定，millsecond是以毫秒为单位的休眠时间。如果线程在休眠时被打断，JVM就抛出InterruptedException异常。因此，必须在try-catch语句块中调用sleep方法。

### isAlive()

​		判断一线程是否处于活着的状态。线程处在新建状态，在执行start()方法之前，和执行完run()方法之后，线程进入死亡状态，调用isAlive方法，返回false；否则返回true。

### currentThread()

​		currentThread方法是Thread类中的类方法，可以用类名调用，该方法返回当前正在使用CPU资源的线程。

### interrupt()

​		interrupt方法经常用来“吵醒”休眠的线程。当一些线程调用sleep方法处于休眠状态时，一个占有CPU资源的线程可以让休眠的线程的调用interrupt()方法“吵醒”自己，即导致休眠排队等待CPU资源。

**下一节**：将总结线程同步、线程间通信、线程死锁、线程联合等有关概念。

