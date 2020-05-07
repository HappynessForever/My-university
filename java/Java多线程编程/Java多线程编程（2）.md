[TOC]

# Java多线程编程（2）

## 互斥同步

​		所谓线程同步就是若干个线程都需要使用一个**synchronized**（同步）修饰的方法，即程序中的若干个线程都需要使用**同一个方法**，而这个方法用synchronized给与了修饰。

​		Java提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是JVM实现的**synchronized**，而另一个是JDK实现的**ReentrantLock**。

```java
Lock lock = new ReentrantLock();
```

## 线程之间的协作

​		当多个线程可以一起工作去解决某个问题时，如果某些部分必须在其它部分之前完成，那么就需要对线程进行协调。

### wait() 	notify()	notifyAll()

​		它们都**属于Object**的一部分，而不属于 Thread。调用 wait() 使得线程等待某个条件满足，线程在等待时会被**挂起**，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来**唤醒**挂起的线程。只能用在**同步方法**或者**同步控制块**中使用，否则会在运行时抛出 IllegalMonitorStateException。使用 wait() 挂起期间，线程会**释放锁**。

**wait() 和 sleep() 的区别**

wait() 是Object的方法，而sleep() 是Thread的静态方法

wait() 会释放锁，sleep() 不会。

### join()

​		一个线程A在占有CPU资源期间，可以让其他线程调用join() 和本线程联合，如：B.join()；

解释：我们称A在运行期间联合了B。如果线程A在占有CPU资源期间一旦联合B线程，那么A线程将会立刻中断执行，一直等到它联合的线程B执行完毕，A线程再重新排队等待CPU资源，以便恢复执行。如果A准备联合的B线程已经结束，那么B.join()不会产生任何效果。

## 守护线程

​		线程默认是非守护线程，非守护线程也称作用户（user）线程，一个线程在执行之前调用void **setDaemon(boolean on)**方法来设置自己是否为守护线程。当程序中的所有用户线程都已经结束运行时，守护线程不管有没有运行完成，也会结束。我们可以使用守护线程做一些不是很严格的工作，线程的随时结束不会产生什么不良后果。

## 线程死锁

**同步弊端**：效率低，如果出现了同步嵌套，就容易产生死锁问题。

**死锁问题**：是指两个或两个以上的线程在执行的过程中，因竞争资源产生的一种互相等待现象。

```java
public class 死锁问题 {
	private static Object lock1 = new Object();
	private static Object lock2 = new Object();
	
	public static void main(String[] args) {
		new Thread(new Runnable() {
			public void run() {
				synchronized (lock1) {
					System.out.println("thread1 get lock1");
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					synchronized (lock2) {
						System.out.println("thread1 get lock2");
					}
				}
			}
		}).start();

		new Thread(new Runnable() {
			public void run() {
				synchronized (lock2) {
					System.out.println("thread2 get lock1");
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					synchronized (lock1) {
						System.out.println("thread2 get lock2");
					}
				}
			}
		}).start();
	}
}
```

程序输出：

```
thread1 get lock1
thread2 get lock2
```

