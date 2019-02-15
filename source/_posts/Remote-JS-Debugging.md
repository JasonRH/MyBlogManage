---
title: android模拟器与chrome进行Remote JS Debugging连接不上
date: 2019-01-24 14:59:09
categories: 前端
tags: [react-native]
---

#### 现象

在Android模拟器上进行 Debug JS Remotely时，Chrome浏览器会自动打开http://10.0.2.2:8081/debugger-ui ，但此网址无法访问。

#### 解决方法

1. 在模拟器上`Ctrl + M` 进入开发者菜单

2. Dev settings > Debug server host & port for device

3. 设置 `localhost:8081`

4. 重新运行 `react-native run-android`

#### 真机调试

##### 在iOS上

打开RCTWebSocketExecutor.m文件，将“localhost”改为你的电脑IP，然后在Developer Menu下单机“Debug JS Remotely”启动JS远程调试。

##### 在Android上

默认情况下不用进行设置就可以进行调试，当报错时进行以下方法。

方法一：在Android5.0以上设备，将手机通过usb连接到电脑上，然后输入以下adb命令来设置端口转发

```powershell
adb reverse tcp:8081 tcp:8081
```

方法二：通过在“Developer Menu”下的“Dev Settings”中设置你的电脑ip来进行调试（保证他们在同一个路由器下）
