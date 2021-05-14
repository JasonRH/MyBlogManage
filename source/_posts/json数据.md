---
title: json数据
categories: 移动端
tags:
  - json
abbrlink: 49826
date: 2019-07-10 13:10:05
---
1.出现转译字符    
    
    String s = new Gson().toJson(encodedText);出现转译字符
    
    原因：Gson会把html标签，转换为Unicode转义字符。
    
    正确的使用方法是:Gson gson = new GsonBuilder().disableHtmlEscaping().create();