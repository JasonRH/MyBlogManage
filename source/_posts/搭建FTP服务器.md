---
title: 搭建FTP服务器
categories: 后端
tags:
  - 服务器
abbrlink: 26945
date: 2019-12-25 19:41:24
---

1.安装
[root@Sungeek ~]# yum -y install vsftpd

1.编辑配置
[root@Sungeek ~]# cd /etc/vsftpd
[root@Sungeek ~]# vim vsftpd.conf

```
//本地用户访问
local_enable=YES
#本地用户访问目录
local_root=/home/FTP/
#只允许在主目录下活动
chroot_local_user=YES

#匿名用户不能访问
anonymous_enable=NO
#匿名用户访问目录
anon_root=/home/RH/JasonRH.github.io/index.html

```



3.
添加FTP用户命令：useradd XXX
设置FTP用户密码：passwd XXX
设置用户目录访问权限：
[root@Sungeek vsftpd]# chown -R ftpuser /home/ftp

3、设置vsftpd服务开机启动
[root@Sungeek ~]# systemctl enable vsftpd.service

//设置开机自启动
systemctl enable vsftpd.service                     
//启动ftp服务
systemctl start vsftpd.service
//暂停ftp服务
systemctl status vsftpd.service
//查看ftp服务端口
netstat -antup | grep ftp               
重启服务
service vsftpd restart


### 开启被动模式
```
pasv_enable=YES # 是否允许数据传输时使用PASV模式（默认值为 YES）
pasv_min_port=port port_number # PASV 模式下，数据传输使用的端口下界（0 表示任意。默认值为 0）把端口范围设在比较高的一段范围内，比如 50000-60000，将有助于安全性的提高.
pasv_max_port=port_number # PASV 模式下，数据传输使用的端口上界（0 表示任意。默认值为 0）
pasv_promiscuous=NO # 是否屏蔽对 PASV 进行安全检查，默认值为 NO（当有安全隧道时可禁用）
pasv_address # PASV 模式中服务器传回的 ip 地址。默认值为 none，即地址是从呼入的连接套接字中获取。
```