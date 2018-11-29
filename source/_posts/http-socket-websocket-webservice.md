---
title: Http、Socket、WebSocket、WebService
date: 2018-11-28 15:49:28
categories: 前端
tags: [协议, 通讯]
---

### 网络分层

![](https://img-blog.csdn.net/20170810213001789?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2t5cm9iZW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

   图片转载自：https://blog.csdn.net/skyroben/article/details/77073834 ，可前往查看原图。

---

### 协议

#### IP 协议

对应于网络层，同样解决数据在网络中的传输。

#### TCP 协议

传输控制协议，对应于传输层。是一种面向连接的、可靠的、基于[字节流](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AF%80%E6%B5%81 "字节流")的[传输层](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E5%B1%82 "传输层")通信协议，主要解决数据在网络中的传输。使用**三次握手**协议建立连接，进行数据重发，保证了连接的可靠性。理论上，TCP 连接一旦建立，在通讯双方中的任何一方主动断开连接之前 TCP 连接会一直保持下去。

#### UDP协议

是一个简单的面向数据报的[传输层](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E5%B1%82 "传输层")协议。在网络中它与 TCP 协议一样用于处理数据包，是一种无连接的协议。传送数据前并不与对方建立连接，对接收到的数据也不发送确认信号，发送端不知道数据是否会正确接收，当然也不用重发。

#### TCP/IP 协议

即**互联网协议**，是互联网相关的各类协议族的总称，比如：TCP，UDP，IP，FTP，HTTP，ICMP，SMTP 等都属于 TCP/IP 族内的协议。因为该协议家族的两个核心协议：TCP（[传输控制协议](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE "传输控制协议")）和IP（[网际协议](https://zh.wikipedia.org/wiki/%E7%BD%91%E9%99%85%E5%8D%8F%E8%AE%AE "网际协议")），为该家族中最早通过的标准。为了突出 TCP 与 IP 这两个协议的重要性，所以就用 TCP/IP 来表示 TCP/IP 协议族了。

#### HTTP 协议

超文本传输协议，对应于应用层，用于如何封装数据。  **HTTP为短连接：**客户端发送请求都需要服务器端回送响应.请求结束后，主动释放链接，因此为短连接。

#### 补充

1. 我们在传输数据时，可以只使用(传输层)TCP/IP协议，但是那样的话，如果没有应用层，便无法识别数据内容。

2. TCP 与 UDP 的区别

   TCP 用于在传输层有必要实现可靠传输的情况。由于它是面向有链接并具备顺序控制、重发控制等机制的，所以他可以为应用提供可靠的传输。 而在一方面，UDP 主要用于那些对高速传输和实时性有较高要求的通信或广播通信。 我们举一个通过 IP 电话进行通话的例子。如果使用 TCP，数据在传送途中如果丢失会被重发，但这样无法流畅的传输通话人的声音，会导致无法进行正常交流。而采用 UDP，他不会进行重发处理。从而也就不会有声音大幅度延迟到达的问题。即使有部分数据丢失，也支持会影响某一小部分的通话。此外，在多播与广播通信、视频中也是用 UDP 而不是 TCP。

---

### 什么是Socket？

Socket是对TCP/IP协议的封装，Socket本身并不是协议，而是一个调用接口（API），通过Socket，我们才能使用TCP/IP协议。实际上，Socket跟TCP/IP协议没有必然的联系。Socket编程接口在设计的时候，就希望也能适应其他的网络协议。所以说，Socket的出现只是使得程序员更方便地使用TCP/IP协议栈而已，是对TCP/IP协议的抽象，从而形成了我们知道的一些最基本的函数接口，比如create、listen、connect、accept、send、read和write等等。网络有一段关于socket和TCP/IP协议关系的说法比较容易理解：“TCP/IP只是一个协议栈，就像操作系统的运行机制一样，必须要具体实现，同时还要提供对外的操作接口。这个就像操作系统会提供标准的编程接口，比如win32编程接口一样，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口。”

CSDN上有个比较形象的描述：HTTP是轿车，提供了封装或者显示数据的具体形式；Socket是发动机，提供了网络通信的能力。

**Socket连接为长连接：**通常情况下Socket 连接就是 TCP 连接（Socket当然也可以进行UDP无连接通讯），因此 Socket 连接一旦建立,通讯双方开始互发数据内容，直到双方断开连接。在实际应用中，由于网络节点过多，在传输过程中，会被节点断开连接，因此要通过轮询高速网络，保证该节点处于活跃状态。

若双方是 Socket 连接，可以由服务器直接向客户端发送数据。

若双方是 HTTP 连接，则服务器需要等客户端发送请求后，才能将数据回传给客户端。

---

### 什么是 WebSocket？

WebSocket是HTML5规范提出的一种在单个TCP连接上进行全双工通信的应用层协议。它和HTTP一样都是基于TCP的。

不同的是HTTP是单向的、短连接，而WebSocket是双向的、长连接。

在WebSocket中，只需要服务器和浏览器通过HTTP协议进行一个握手的动作，然后单独建立一条TCP的通信通道进行全双工通信。但是建立之后，在真正传输时候是不需要HTTP协议的。

![图片来源:https://blog.csdn.net/SL_ideas/article/details/73648378](https://img-blog.csdn.net/20170626140009906?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU0xfaWRlYXM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

---

### 什么是 WebService？

WebService是一种跨编程语言和跨操作系统平台的远程调用技术。

所谓跨编程语言和跨操作平台，就是说服务端程序采用java编写，客户端程序则可以采用其他编程语言编写，反之亦然！跨操作系统平台则是指服务端程序和客户端程序可以在不同的操作系统上运行。

从表面上看，WebService就是一个应用程序向外界暴露出一个能通过Web进行调用的API，也就是说能用编程的方法通过Web来调用这个应用程序。我们把调用这个WebService的应用程序叫做客户端，而把提供这个WebService的应用程序叫做服务端

WebService通过HTTP协议发送请求和接收结果的，但相比HTTP只传输字符串,它发送的请求内容和结果内容都采用XML格式封装，并增加了一些特定的HTTP消息头，以说明HTTP消息的内容格式，这些特定的HTTP消息头和XML内容格式就是SOAP协议。SOAP提供了标准的RPC方法来调用Web Service。

SOAP协议 = HTTP协议 + XML数据格式

---

### 参考文章

- http://lib.csdn.net/article/computernetworks/20534

- https://www.jianshu.com/p/a18a5ba78fad

- https://www.jianshu.com/p/219eb040479b

- https://blog.csdn.net/SL_ideas/article/details/73648378

- https://blog.csdn.net/chaoshenzhaoxichao/article/details/79785318

- https://zh.wikipedia.org/wiki/TCP/IP%E5%8D%8F%E8%AE%AE%E6%97%8F

- https://www.jianshu.com/p/49d7997ad3b7
