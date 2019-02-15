---
title: InputStream数据读取
date: 2019-02-14 11:13:53
categories: 其它
tags: [Java]
---

```java
  int data0 = inputStream.read();
```

· ***read()*** : 从(来源)输入流中(读取的内容)读取数据的下一个字节到(去处)java程序内部中返回值为0到255的int类型的值，返回值为字符的ASCII值(如a就返回97,n就返回110)。  如果没有可用的字节,因为已经到达流的末尾, -1返回的值。运行一次只读一个字节,所以经常与while((len = inputstream.read()) != -1)一起使用



```java
byte[] buffer=new byte[512];
int size = mInputStream.read(buffer);
```

· ***read(byte[] b)***：从输入流中读取一定数量的字节，并将其存储在缓冲区数组b 中。以整数形式返回实际读取的字节数。在输入数据可用、检测到文件末尾或者抛出异常前，此方法一直阻塞。

· 如果 b 的长度为 0，则不读取任何字节并返回 0；否则，尝试读取至少一个字节。如果因为流位于文件末尾而没有可用的字节，则返回值 -1；否则，至少读取一个字节并将其存储在 b *中。

· 将读取的第一个字节存储在元素b[0] 中，下一个存储在 b[1] 中，依次类推。读取的字节数最多等于b 的长度。设 k为实际读取的字节数；这些字节将存储在b[0] 到 b[k-1] 的元素中，不影响 b[k] 到b[b.length-1] 的元素。





```java
int newcount = inputStream.read(b, off, len);
```

· ***read(byte[] b, int off, int len)***：读取 len字节的数据从输入流到一个字节数组。

· 试图读取多达 len字节,但可能读取到少于len字节。返回实际读取的字节数为整数。

· 第一个字节存储读入元素b[off],下一个b[off+1],等等。读取的字节数是最多等于len。k被读取的字节数,这些字节将存储在元素通过b[off+k-1]b[off]，离开元素通过b[off+len-1]b[off+k]未受影响。

· read(byte[]b)就是相当于read(byte [] b , 0 , b.length).所以两者差不多，性质一样。
