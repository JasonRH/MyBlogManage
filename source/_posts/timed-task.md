---
title: 定时任务和倒计时
date: 2018-11-27 13:24:44
categories: 前端
tags: [Java, Android]
---

### Java 基本的定时任务

 总结方法有三种：

1. 创建一个thread，然后让它在while循环里一直运行着，通过sleep方法来达到定时任务的效果；

```java
package com.rh.demo;

public class JavaTimerTest1 {
    public static void main(String[] args) {
        final long timeInterval = 1000;
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                while (true) {
                    System.out.println("Hello");
                    try {
                        Thread.sleep(timeInterval);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        };
        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```

2. 用Timer和TimerTask

   与第一种方法相比有如下好处：

   - 当启动和去取消任务时可以控制
   - 第一次执行任务时可以指定你想要的delay时间

   ```java
   package com.rh.demo;
   
   import java.util.Timer;
   import java.util.TimerTask;
   
   public class JavaTimerTest2 {
   
      public static void main(String[] args) {
          TimerTask timerTask = new TimerTask() {
              @Override
              public void run() {
                  System.out.println("Hello");
              }
          };
          Timer timer = new Timer();
          long delay = 0;
          long intevalPeriod = 1 * 1000;
          timer.scheduleAtFixedRate(timerTask,delay,intevalPeriod);
      }
   }
   ```

3. 用**ScheduledExecutorService**

   做为并发工具类被引进的，这是最理想的定时任务实现方式，相比于上两个方法，它有以下好处：

   - 相比于Timer的单线程，它是通过线程池的方式来执行任务的
   - 可以很灵活的去设定第一次执行任务delay时间
   - 提供了良好的约定，以便设定执行的时间间隔

```java
package com.rh.demo;

import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class JavaTimerTest3 {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello");
            }
        };
        //手动创建线程池，效果会更好哦，不推荐使用以下创建线程池方法,详情见博客中‘线程池’一文

        ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
        service.scheduleAtFixedRate(runnable, 0, 1, TimeUnit.SECONDS);
    }
}
```

方法说明：

1. schedule(commod,delay,unit) ，这个方法是说系统启动后，需要等待多久执行，delay是等待时间。只执行一次，没有周期性。  

2. scheduledThreadPool.scheduleAtFixedRate(new TaskTest("执行调度任务3"),0, 1, TimeUnit.SECONDS);   

   //这个就是每隔1秒，开启一个新线程，不管上一次是否执行完 

3. scheduledThreadPool.scheduleWithFixedDelay(new TaskTest("第四个"),0, 3, TimeUnit.SECONDS); 

   //这个就是上一个任务执行完，3秒后开启一个新线程

---

### Android中独有的定时任务

1. Handler

   ```java

   /** 

   * handler定时器使用postDelyed实现 
   * 
   * @author Smalt 
   * */  

     public class HanderDemoActivity extends Activity {  

     TextView tvShow;  

     private int i = 0;  

     private int TIME = 1000;  

     @Override  

     public void onCreate(Bundle savedInstanceState) {  

        super.onCreate(savedInstanceState);  

        setContentView(R.layout.main);  

        tvShow = (TextView) findViewById(R.id.tv_show);  

        handler.postDelayed(runnable, TIME); //每隔1s执行  

     //handler.removeCallbacks(runnable);  //关闭计时器

     }  

     Handler handler = new Handler();  

     Runnable runnable = new Runnable() {  

        @Override  

        public void run() {  

            // handler自带方法实现定时器  
            try {  
                handler.postDelayed(this, TIME);  
                tvShow.setText(Integer.toString(i++));  
                System.out.println("do...");  
            } catch (Exception e) {  
                // TODO Auto-generated catch block  
                e.printStackTrace();  
                System.out.println("exception...");  
            }  

        }  

     };  

     }  

```

2. AlarmManager

```java
//每隔5s运行一次HorizonService
public class HorizonService extends Service {
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                Log.d("TAG", "打印时间: " + new Date().toString());

            }
        }).start();
        AlarmManager manager = (AlarmManager) getSystemService(ALARM_SERVICE);
        int five = 5000; // 这是5s
        long triggerAtTime = SystemClock.elapsedRealtime() + five;
        Intent i = new Intent(this, HorizonService.class);
        PendingIntent pi = PendingIntent.getBroadcast(this, 0, i, 0);
        manager.set(AlarmManager.ELAPSED_REALTIME_WAKEUP, triggerAtTime, pi);
        return super.onStartCommand(intent, flags, startId);
    }
}
```

---

### 倒计时

1. **简易方式实现**

```java
import java.util.*;  

import java.util.concurrent.*;  



public class CountDown {  

       private int limitSec;  

       public CountDown(int limitSec) throws InterruptedException{  

           this.limitSec = limitSec;  

           System.out.println("Count from "+limitSec);  

           while(limitSec > 0){  

               System.out.println("remians "+ --limitSec +" s");  

               TimeUnit.SECONDS.sleep(1);  

           }  

           System.out.println("Time is out");  

    }  
}
```

1. **使用ScheduleExecutor实现**

   ```java

   import java.util.concurrent.*;  

   public class CountDown1 {  

          private int curSec;   //记录倒计时当下时间  
          public CountDown1(int limitSec) throws InterruptedException{  
       
              this.curSec = limitSec;  
              System.out.println("count down form "+limitSec);  
       
              //手动创建线程池，效果会更好哦，不推荐使用以下创建线程池方法   
       
              ScheduledExecutorService exec = Executors.newScheduledThreadPool(1);  
       
              exec.scheduleAtFixedRate(new Task(),0,1,TimeUnit.SECONDS);  
       
              TimeUnit.SECONDS.sleep(limitSec);//sleeping for limitSec seconds 
       
              exec.shutdownNow();  
       
              System.out.println("Time out！");  
       
          }  
       
          private class Task implements Runnable{
             @Override  
       
              public void run(){  
       
                  System.out.println("Time remains "+ --curSec +" s");  
       
              }  
       
          }  

}  

```
2.  **使用java.util.Timer实现**

   ```java
   import java.util.*;  

   import java.util.concurrent.TimeUnit;  



   public class CountDown2 {  

          private int curSec;  
          public CountDown2(int limitSec) throws InterruptedException{  

              this.curSec = limitSec;  
              System.out.println("count down from "+limitSec+" s ");  

              Timer timer = new Timer();  

              timer.schedule(new TimerTask(){  

                  public void run(){  

                      System.out.println("Time remians "+ --curSec +" s");  

                  }  

              },0,1000);  

              TimeUnit.SECONDS.sleep(limitSec);//sleeping for limitSec seconds  

              timer.cancel();  

              System.out.println("Time is out!");  

          }    

   }
```

3. Android特有

   ```java
   new CountDownTimer(10000, 1000) {
       @Override
       public void onTick(long millisUntilFinished) {
           //textview.setText("倒计时 "+(millisUntilFinished / 1000) + "秒"); 
       }
       @Override
       public void onFinish() {
   
       }
   }.start();
   ```
