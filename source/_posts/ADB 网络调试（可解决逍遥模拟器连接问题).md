---
title: ADB 网络调试（可解决逍遥模拟器连接问题）
categories: 其它
tags:
  - Android
abbrlink: 46373
date: 2020-11-11 10:56:08
---

```
1.数据线连接手机到电脑（确保手机打开了USB调试功能，逍遥模拟器需要连接成功一次）

2.AS Terminal中输入命令 adb tcpip 5555 ,执行成功后可以断开数据线（确保命令行中可以调用到adb,可能需要配置环境变量）

3.AS Terminal中输入命  adb connect 手机ip:5555 ,  例如我的是：adb connect 172.16.55.24:5555

关机开启时仍然有效（IP未变的前提下）
```