---
title: Git乱码
categories: 其他
tags:
  - git
abbrlink: 57920
date: 2019-08-06 09:40:23
---

解决git bash 终端显示中文乱码
要注意的是，这样设置后，你的git bash终端也要设置成中文和utf-8编码。才能正确显示中文，例如对比如下：



在git bash的界面中右击空白处，弹出菜单，选择选项->文本->本地Locale，设置为zh_CN，而旁边的字符集选框选为UTF-8。

英文显示则是： 
Options->Text->Locale改为zh_CN，Character set改为UTF-8



通过修改配置文件来解决中文乱码
如果你的git bash终端没有菜单选项显示，还可以通过直接修改配置文件的方式来解决中文乱码问题。

进入git的安装目录

编辑etc\gitconfig文件，也有些windows系统是存放在C:\Users\Administrator\.gitconfig路径或安装盘符:\Git\mingw64\etc\gitconfig，在文件末尾增加以下内容：
[gui]  
    encoding = utf-8  
    # 代码库统一使用utf-8  
[i18n]  
    commitencoding = utf-8  
    # log编码  
[svn]  
    pathnameencoding = utf-8  
    # 支持中文路径  
[core]
    quotepath = false 
    # status引用路径不再是八进制（反过来说就是允许显示中文了）

--------------------- 
版权声明：本文为CSDN博主「铁乐与猫」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u012145252/article/details/81775362