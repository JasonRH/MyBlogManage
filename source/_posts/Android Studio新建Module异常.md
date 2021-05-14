---
title: Android Studio新建Module异常
categories: 移动端
tags:
  - Android Studio
abbrlink: 46280
date: 2019-12-10 09:40:23
---

#### 问题：
新建module后，上面显示 Gradle sync 同步失败，build窗口一直卡在同步中，卸载重装 更换sdk，删除.gradle和 android配置文件夹都没有用

###  解决办法：
由于开启了渐变同步导致，到设置界面：   File → Settings → Experimental → Gradle 把Only sync the active variant 的勾去掉即可

![取消打勾](https://img-blog.csdnimg.cn/20191210093212747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dQUjEzMDA1NjU1OTg5,size_16,color_FFFFFF,t_70)