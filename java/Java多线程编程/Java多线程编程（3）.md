# Java多线程编程（3）

​		**线程池**里的每一个线程代码结束后，并**不会死亡**，而是再次回到线程初中成为**空闲状态**，等待下一个对象来使用。此时程序不会结束。

```java
package concurrency;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/*
 * pubilc static ExecutorService newFixedThreadPool(int nThreads)
 * 这种线程池的线程可以执行：Runnable对象或者Callable对象代表的线程
 * 如果想要线程返回一个结果就用Callable（实现要依赖线程池），重点掌握前两中创建线程的方法
 */
public class ExcutorsDemo {
	public static void main(String[] args) {
		ExecutorService pool = Executors.newFixedThreadPool(2);	//指定线程池的容量为2
		//添加线程
		pool.submit(new SS());	
		pool.submit(new SS());	
	}
}

class SS implements Runnable {

			@Override
			public void run() {
				for(int i=0; i<100; i++) {
					System.out.println(Thread.currentThread().getName() + ":" + i);
				}
				
			}
			
		}
```

## 匿名内部类的方式实现多线程程序

```java
public class ThreadDemo {
    public static void main(String[] args){
        //使用Thread类来实现多线程
        new Thread() {
            public void run(){
                for(int i=0; i<100; i++){
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                }
            }
        }.start();
        
        //实现Runnable接口实现线程
        new Thread(new Runnable(){
            public void run(){
                for(int i=0; i<100; i++){
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                }
            }
        }).start();
    }
}
```

