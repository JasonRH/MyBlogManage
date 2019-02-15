---
title: 线程池
date: 2018-11-28 13:20:25
categories: 前端
tags: [Java, 线程池]
---

##### 说明

标记以下比较重要的类：

| ExecutorService             | 真正的线程池接口                                                      |
| --------------------------- | ------------------------------------------------------------- |
| ScheduledExecutorService    | 能和Timer/TimerTask类似，解决那些需要任务重复执行的问题                           |
| ThreadPoolExecutor          | ExecutorService的默认实现                                          |
| ScheduledThreadPoolExecutor | 继承ThreadPoolExecutor，实现ScheduledExecutorService接口，周期性任务调度的类实现 |

线程资源必须通过线程池提供，不允许在应用中自行显式创建线程(例如：不能直接new Thread(){}.start();))。 说明：使用线程池的好处是减少在创建和销毁线程上所花的时间以及系统资源的开销，解决资源不足的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致消耗完内存或者“过度切换”的问题。

线程池不允许使用Executors 去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗费的风险。

说明： Executors各个方法的弊端：

1） newFixedThreadPool和newSingleThreadExecutor:

主要问题是堆积的请求处理队列可能会耗费很大的内存，甚至OOM。

2） newCachedThreadPool和newScheduledThreadPool

主要问题是线程数最大是Integer.MAX_VALUE,可能会创建数量非常多的线程，甚至OOM

---

推荐使用方法

##### 单线程示例

```java
//线程池命名
ThreadFactory namedThreadFactory = new ThreadFactoryBuilder()
                .setNameFormat("demo-pool-%d")
                .build();
//创建线程池                

        ExecutorService singleThreadPool = new ThreadPoolExecutor(
                1,
                1,
                0L, 
                TimeUnit.MILLISECONDS,
                new LinkedBlockingQueue<Runnable>(1024), 
                namedThreadFactory, 
                new ThreadPoolExecutor.AbortPolicy()
                );

        singleThreadPool.execute(() -> System.out.println(Thread.currentThread().getName()));
        singleThreadPool.shutdown();
```

##### 周期性线程示例

```java
//创建定时任务线程

ScheduledExecutorService schedualExec = new ScheduledThreadPoolExecutor(1);

    ScheduledFuture downData = schedualExec.scheduleWithFixedDelay(task, 1000, 3000, TimeUnit.  MILLISECONDS );

    Runnable task = () -> {
        System.out.println("上传1:" + new Date());
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    };

    public void close() {
        if (schedualExec != null && !schedualExec.isShutdown()) {
            schedualExec.schedule(new Runnable() {
                @Override
                public void run() {
                    if (downData != null) {
                        downData.cancel(true);
                    }
                }
            }, 1, TimeUnit.SECONDS);

            schedualExec.shutdownNow();
        }

    }
```
