---
title: Replugin插件化
categories: 前端
tags:
  - Android
abbrlink: 35111
date: 2021-03-24 11:37:00
---

### 1.概念及技术分析
```
Atlas:功能强大，完善。学习成本，接入成本比较高，适合门户型大型应用。

Replugin：轻量级可快速使用的插件化框架，适合中小型应用。
优点：API接近原生应用，只有一个hook点，兼容性好，不需要随着Android系统的升级进行后续的兼容。
```

 ### 2.学习总结

```
1.getMergeAssetTask()不到
解决： android-gradle-plugin:3.5.3
       replugin-host-gradle:2.3.3

2.插件生成步骤:
          1.正确配置repluginPluginConfig{}
          2.Activity的正确继承
          3../gradlew :rh_login:assembleDebug
          4.改名为rh_login.jar
          5.放到宿主工程中正确的位置

3.宿主跳转插件activity
      Intent intent = RePlugin.createIntent("rh_login","com.example.LoginActivity");
      intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
      RePlugin.startActivity(this,intent);

4.插件化开发广播发送和接收：与普通app开发完全一样，唯一要注意的是数据的传递

5.插件间AIDL接口通讯：
             1.AIDL接口中本身注意的一些事项
             2.AIDL接口实现在对应的插件application中注册
             3.通过RePlugin.fetchBinde获取对象去使用

6.插件中启动前台services？RePlugin2.3.3版本还不支持,后台service正常，未来会提供

7.插件间activity跳转不支持转场动画

8.插件间fragment调用，直接通过RePlugin查找会报ClassCastException
原因：不同classloader加载同一份类字节码，还是不同的class，所以他们的实例无法强转
解决：通过compileOnly欺骗编译器，最总都使用宿主工程引入的类

9.插件间无法去直接对外暴露View
```