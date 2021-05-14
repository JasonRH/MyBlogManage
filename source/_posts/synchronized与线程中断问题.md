---
title: synchronized与线程中断问题
categories: 其它
tags:
  - Android
abbrlink: 16996
date: 2021-03-25 11:30:00
---

```
public class Uninterruptible {
    private static final Object o1 = new Object();

    public static void main(String[] args) throws InterruptedException {
    
        Thread thread1 = new Thread(() -> {
            synchronized (o1) {
                try {
                    System.out.println("start lock t1");
                    Thread.sleep(20000);
                    System.out.println("end lock t1");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        Thread thread2 = new Thread(() -> {
            synchronized (o1) {
              while (!isInterrupted()) {
                 try {
                    System.out.println("start lock t2");
                    Thread.sleep(20000);
                    System.out.println("end lock t2");
                  } catch (InterruptedException e) {
                    e.printStackTrace();
                    //重新设置中断标示,因为抛出异常后中断标示会被清除。要想线程停止需要加上这一句
                    Thread.currentThread().interrupt();
                }
             }
           }
        });
        

        thread1.start();
        // 中断线程的执行
        thread1.interrupt();
        //中断线程2
        thread2.interrupt();
        //锁死情况下，interrupt无效。
    }
}

总结：
1.要想中断线程需要在catch子句中，调用Thread.currentThread.interrupt()来设置中断状态（因为抛出异常后中断标示会被清除），让外界通过判断Thread.currentThread().isInterrupted()标示来决定是否终止线程还是继续下去，否者线程阻塞状态下是不会中断的。
2.synchronized在获锁的过程中是不能被中断的，意思是说如果产生了死锁，则不可能被中断。与synchronized功能相似的reentrantLock.lock()方法也是一样，它也不可中断的，即如果发生死锁，那么reentrantLock.lock()方法无法终止，如果调用时被阻塞，则它一直阻塞到它获取到锁为止。
3.线程管理推荐使用线程池，不直接使用 new Thread()。

```