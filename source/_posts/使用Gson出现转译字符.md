---
title: 使用Gson出现转译字符
tags:
  - json
abbrlink: 22247
date: 2021-03-30 13:10:05
---
#### 1.出现转译字符    
    
    String s = new Gson().toJson(encodedText);出现转译字符
    
    原因：Gson会把html标签，转换为Unicode转义字符。
    
    正确的使用方法是:Gson gson = new GsonBuilder().disableHtmlEscaping().create();
######    注意：用AES加密后的字符串转义后会变
    
    
#### 2.pache工具包common-lang进行html,xml,java等的转义与反转义
```
String str1 = StringEscapeUtils.unescapeJava(str);
原始 str = {\"name\":\"spy\",\"id\":\"123456\"}
目标 str1 = {"name":"spy","id":"123456"}
```
下载地址：https://commons.apache.org/proper/commons-lang/download_lang.cgi
