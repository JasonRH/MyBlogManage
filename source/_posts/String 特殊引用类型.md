---
title: String特殊引用类型
categories: 前端
tags:
  - Java
abbrlink: 5952
date: 2021-03-05 11:25:00
---

String为值类型还是引用类型？

```
//值类型
int a = 1;
int b = a;
a = 2;
Console.WriteLine("a is {0},b is {1}", a, b);

//字符串
string str1 = "ab";
string str2 = str1;
str1 = "abc";
Console.WriteLine("str1 is {0},str2 is {1}", str1, str2);

输出结果：
//a is 2,b is 1
//str1 is abc,str2 is ab
str2依然是ab,并没有随str1的改变而改变。

如果string是引用类型，按理Str1和Str指针都指向同一内存地址，如果Str的内容发生改变，Str1应该也会相应变化。

此例子，看着string更像是值类型。
```

```
引用类型例子：

String aa = new String("AA");这样定义，aa就是一个引用
StringBuilder strb2 = new StringBuilder("BB");
```

结论：

String是引用类型，只是编译器对其做了特殊处理。