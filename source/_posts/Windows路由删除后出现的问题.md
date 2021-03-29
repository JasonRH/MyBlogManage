---
title: 'Windows路由删除后出现的问题'
categories: 其它
tags:
  - Android
abbrlink: 44730
date: 2019-04-24 11:08:30
---

错误:Protocol family unavailable和connect:network is unreachable

原由：在Windows中 进行路由重置操作 route -f

现象：
1.Android Studio 项目无法编译，报错 ERROR: Protocol family unavailable
2.路由删除后重新连上网，Shadowsocks无法翻墙，国内网络可以上

解决：
1.参照网上经验 关防火墙，然后重启as，没用。
2.在环境变量添加：_JAVA_OPTIONS  值为：-Djava.net.preferIPv4Stack=true 
此时Android Studio出现新的错误connect: network is unreachable ,还是无法翻墙
3.以管理员身份运行 命令提示符，键入netsh winsock reset，再重启电脑 。正解，问题解决！
