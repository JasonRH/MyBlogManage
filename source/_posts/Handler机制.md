---
title: Handler机制
categories: 前端
tags:
  - Android
abbrlink: 6248
date: 2021-04-08 11:15:00
---

整个消息的循环流程:
-  1.Handler 通 过 sendMessage() 发 送 消 息 Message 到 消 息 队 列 MessageQueue。
- 2.Looper 通过 loop()不断提取触发条件的 Message，并将 Message 交 给对应的 target handler 来处理。
-  3.target handler 调用自身的handleMessage()方法来处理 Message。


结论：
- 主线程创建的时候会创建一个MainLooper，并且进入循环。
- 线程的 Looper 不允许退出，ActivityThread的main方法主要就是做消息循环，一旦退出消息循环，那么你的应用也就退出了。
- loop死循环只是简单地处理轻量的消息操作，和ANR并没有关系。
looper.loop() 不断地接收事件、处理事件，每一个点击触摸或者说Activity的生命周期都是运行在 Looper.loop() 的控制之下，如果它停止了，应用也就停止了。只能是某一个消息或者说对消息的处理阻塞了 Looper.loop()，而不是 Looper.loop() 阻塞它。也就说我们的代码其实就是在这个循环里面去执行的，当然不会阻塞了。


[参考链接1](Looper.loop和主线程的关系)

[参考链接2](https://www.cnblogs.com/l2rf/p/6055218.html)