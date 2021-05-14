---
title: ButterKnife在lib中R2方式使用及配置
categories: 移动端
tags:
  - Android
abbrlink: 55063
date: 2021-03-08 19:02:00
---

配置ButterKnife

```
1.在全局的build.gradle中dependencies配置如下代码
 classpath 'com.jakewharton:butterknife-gradle-plugin:*.*.*'

2.在lib build.gradle头部中添加如下代码：
apply plugin: 'com.jakewharton.butterknife'

3.在lib build.gradle 中添加如下依赖，版本根据自己依赖而定，不是唯一
 compile 'com.jakewharton:butterknife:*.*.*'
 annotationProcessor 'com.jakewharton:butterknife-compiler:*.*.*'

4.在lib中是使用ButterKnife ，手动把@bind中的R改成R2，这时候会报红，我们进行rebuild即 可。
```
注意点：library中switch-case的使用，在library中是不能使用switch- case 找id的，解决方法就是用if-else代替。


![image](https://upload-images.jianshu.io/upload_images/6335486-8d2edabe194ba552.png?imageMogr2/auto-orient/strip|imageView2/2/w/378/format/webp)

使用switch-case会报错。
使用if-else还有一点注意。如图


![image](https://upload-images.jianshu.io/upload_images/6335486-6b52caacc75d79ee.png?imageMogr2/auto-orient/strip|imageView2/2/w/425/format/webp)


使用体验：不推荐使用butterknife