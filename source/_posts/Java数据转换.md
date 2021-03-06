---
title: Java数据转换
categories: 后端
tags:
  - 数据转换
abbrlink: 60856
date: 2019-04-08 13:30:35
---

### **基本Java类型转换**

Java类型转换分为自动转换和强制转换两种。



基本类型间的自动类型转换需要满足以下条件:

(1)转换双方的类型必须兼容，例如int和long类型就是兼容的，而int和boolean就是不兼容的。

(2)只能是"窄类型"向"宽类型"转换,也就是目标类型的数据表示范围要比源类型的数据表示范围要大。

byte-->short-->int-->long

float-->double

按照箭头可以实现自动类型转换，而如果是相反方向间的类型转换则需要强制类型转换（强制转换会造成精度缺失）.



### **数值常量默认类型**

(1)Java中整型常量数值的默认类型是int类型，如果需要声明long类型的常量 ，需要在数值加上'l'或者'L'.

例如:int i = 3;

long l = 3L;

(2)Java中的浮点型常量数值默认是double类型，如果要声明一个数值为float型，则需要在数值后面加上'f'或者'F'.

Float = 3.4是错误的 ，高级向低级转换用强转



<font color=#EE1111 size=3>
1 byte (字节)= 8 bit (八个二进制数)
char = 2 byte
short = 2 byte
int = 4 byte
long = 4|8 byte
float = 4 byte
double = 8 bytef
</font>

<font color=#0099FF size=3>
short SF = 0xC8;
int data = 200;
则 data == SF 成立
</font>



### **byte 转 int**

b[i] & 0xFF运算后得出的仍然是个int,那么为何要和 0xFF进行与运算呢? 将byte强转为int不行吗?答案是不行的.

其原因在于:

1. 计算机中数据按照补码存储

2. byte是8位，int是32位，byte转换为int后是32位，如果不和0xff进行与运算，

如果不进行&0xff，那么当一个byte会转换成int时，由于int是32位，而byte只有8位这时会进行补位，

byte=-1 原码 1000 0001 ，反码1111 1110 ，补码1111 1111

转换为32位int时的补码则为 1111 1111 1111 1111 1111 1111 1111 1111 ，呵呵！即0xffffffff，但是这个数是不对的，这种补位就会造成误差。

和0xff相与后，高24比特就会被清0了，结果就对了。

1111 1111 1111 1111 1111 1111 1111 1111&0xff

= 1111 1111 1111 1111 1111 1111 1111 1111 & 0000 0000 0000 0000 0000 0000 1111 1111

= 0000 0000 0000 0000 0000 0000 1111 1111 (补码)

->0000 0000 0000 0000 0000 0000 1111 1111（原码）= 255（int 十进制）

<font color=#EE1111 size=3>
byte为负数，高3字节就填充1，整数就补0，所以，如果byte是正数那么是否进行&0xff结果都一样；如果是负数就一定需要&0xff
</font>



### **计算机基础理论**

计算机运算时是将原码转成补码，用补码进行运算，最后再将运算结果转换成原码



byte是一个字节保存的，有8个位，即8个0、1。

8位的第一个位是符号位，

也就是说0000 0001代表的是数字1

1000 0001代表的就是-1

所以正数最大位0111 1111，也就是数字127

负数最大为1111 1111，也就是数字-128

上面说的是二进制原码，但是在java中采用的是补码的形式，下面介绍下什么是补码

1、反码：

 一个数如果是正，则它的反码与原码相同；

 一个数如果是负，则符号位为1，其余各位是对原码取反；

2、补码：利用溢出，我们可以将减法变成加法

 对于十进制数，从9得到5可用减法：

 9－4＝5 因为4+6＝10，我们可以将6作为4的补数

 改写为加法：

 9+6＝15（去掉高位1，也就是减10）得到5.

 对于十六进制数，从c到5可用减法：

 c－7＝5 因为7+9＝16 将9作为7的补数

 改写为加法：

 c+9＝15（去掉高位1，也就是减16）得到5.

 在计算机中，如果我们用1个字节表示一个数，一个字节有8位，超过8位就进1，在内存中情况为（100000000），进位1被丢弃。

 ⑴一个数为正，则它的原码、反码、补码相同

 ⑵一个数为负，刚符号位为1，其余各位是对原码取反，然后整个数加1

- 1的原码为 10000001

- 1的反码为 11111110

  + 1

- 1的补码为 11111111

0的原码为 00000000

0的反码为 11111111（正零和负零的反码相同）

 +1

0的补码为 100000000（舍掉打头的1，正零和负零的补码相同）
