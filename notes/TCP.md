---
tags: [TCP]
title: TCP
created: '2019-09-03T03:39:27.065Z'
modified: '2019-09-03T09:29:40.496Z'
---

# TCP

## **TCP概述**

　　• tcp是一个点对点端到端的传输协议，有一个发送方和接收方。

　　• tcp传输的是可靠的按序到达的字节流

　　• tcp采用流水线机制，提高传输的效率。TCP通过拥塞控制和流量控制机制来控制滑动窗口的大小

　　• tcp协议分别设置了发送方缓存和接收方缓存

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/1234352-5614950e073d8a2f.png)

　　• tcp采取全双工(full\-duplex)传输，也就是传输过程中，同一连接可以传输双向的数据流，发送方可以传给接收方，接收方也可以传给发送方。

　　• tcp是面向连接的协议，通信双方在发送数据之前必须建立连接。连接状态只在连接的两端中维护，在沿途节点中并不维护状态。TCP连接包括：两台主机上的缓存、连接状态变量、socket等

　　• tcp实现了流量控制机制

## TCP段结构

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143012.jpg)

　　TCP: 序列号和ACK

　　序列号:

　　• 序列号指的是segment中第一个字节的编号，而不是segment的编号

　　• 建立TCP连接时，双方随机选择序列号

　　ACKs:

　　• 希望接收到的下一个字节的序列号

　　• 累计确认：该序列号之前的所有字节均已被正确接收到

　　Q: 接收方如何处理乱序到达的Segment？

　　• A: TCP规范中没有规定，由TCP的实现者做出决策

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143049.jpg)

　　上图我们进行一个分析，以便搞清楚tcp序列号和ack的应用

　　首先，hostA作为发送方给B发送数据，随机选择一个序列号seq = 42，也就是这段segment中的第一个字节的编号，并且设置ack=79，这表示，希望接收方回传seg=79作为确认信号代表接收方已经正确接受了这段数据

　　然后HostB成功接收到数据，想发送方返回确认信息，根据发送方的ack，所以确认的seg=79，然后通过ack告知希望接收到的下一个字节的序列号，并同时表示之前的所有字节均已被正确接收，所以发送ack=43告知已经接收到43号之前的字节，并希望发送方传送43号字节

## **TCP可靠数据传输**

　　接下来我们看看tcp协议是如何实现可靠传输的。

　　• TCP在IP层提供的不可靠服务基础上实现可靠数据传输服务

　　• 流水线机制

　　• 累积确认

　　• TCP使用单一重传定时器

　　• 触发重传的事件：超时和收到重复ACK

　　**RTT和超时**

　　问题：如何设置定时器的超时时间？

　　• 大于RTT， 但是RTT是变化的

　　• 过短：不必要的重传

　　• 过长： 对段丢失时间反应

　　问题：如何估计RTT？

　　SampleRTT: 测量从段发出去到收到ACK的时间忽略重传

　　SampleRTT变化,测量多个SampleRTT，求平均值，形成RTT的估计值EstimatedRTT

　　EstimatedRTT = (1\- ?)EstimatedRTT + ?SampleRTT

　　指数加权移动平均

　　典型值：0.125

　　**TCP发送方事件**

　　从应用层收到数据后，会进行以下几个步骤：

　　• 创建segment

　　• 序列号是segment第一个字节的编号

　　• 开启计时器

　　• 设置超时时间TimeOutInterval

　　如果发生超时事件：

　　• 重传引起超时的segment

　　• 重启计时器

　　收到ACK：

　　• 如果确认此前未确认的Segment，更新SendBase，如果窗口中还有未被确认的分组，重新启动定时器

　　**发送端伪码**

C++代码

1.  NextSeqNum = InitialSeqNum
2.  SendBase = InitialSeqNum
3.  loop (forever) {
4.  switch(event)
5.  event: data received from application above
6.  create TCP segment with sequence number NextSeqNum
7.  if (timer currently not running)
8.  start timer
9.  pass segment to IP
10.  NextSeqNum = NextSeqNum + length(data)
11.  event: timer timeout
12.  retransmit not\-yet\-acknowledged segment with
13.  smallest sequence number
14.  start timer
15.  event: ACK received, with ACK field value of y
16.  if (y > SendBase) {
17.  SendBase = y
18.  if (there are currently not\-yet\-acknowledged segments)
19.  start timer
20.  }
21.  } /\* end of loop forever \*/

　　**TCP重传示例**

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143737.jpg)

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143706.jpg)

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143637.jpg)

## **快速重传机制**

　　TCP的实现中，如果发生超时，超时时间间隔将重新设置，即将超时时间间隔加倍，导致其很大,重发丢失的分组之前要等待很长时间.

　　通过重复ACK检测分组丢失,Sender会背靠背地发送多个分组,如果某个分组丢失，可能会引发多个重复的ACK.

　　如果sender收到对同一数据的3个ACK，则假定该数据之后的段已经丢失.

　　快速重传：在定时器超时之前即进行重传

　　算法

```
event: ACK received, with ACK field value of y
if (y > SendBase) {
SendBase = y
if (there are currently not-yet-acknowledged segments)
start timer
}
else {
increment count of dup ACKs received for y
if (count of dup ACKs received for y = 3) {
resend segment with sequence number y
}
```

## **TCP流量控制**

　　接收方为TCP连接分配buffer

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/1234352-59ae4bda6636bf08.png)

　　上层应用可能处理buffer中数据的速度较慢

　　流量控制：发送方不会传输的太多、太快以至于淹没接收方（buffer溢出）

　　(假定TCP receiver丢弃乱序的segments)

　　Buffer中的可用空间(spare room)

　　= RcvWindow

　　= RcvBuffer\-\[LastByteRcvd \-LastByteRead\]

　　Receiver通过在Segment的头部字段将RcvWindow 告诉Sender。 Sender限制自己已经发送的但还未收到ACK的数据不超过接收方的空闲RcvWindow尺寸。

　　Receiver告知SenderRcvWindow=0,会出现什么情况？

　　会出现卡死，发送方不发数据了。关于这些问题具体会在tcp拥塞控制里面讨论。

　　**TCP连接管理**

　　TCP sender和receiver在传输数据前需要建立连接。

　　**三次握手机制**

　　Step 1: client host sends TCP SYN

　　segment to server

　　• specifies initial seq #

　　• no data

　　Step 2: server host receives SYN, replies

　　with SYNACK segment

　　• server allocates buffers

　　• specifies server initial seq. #

　　Step 3: client receives SYNACK, replies

　　with ACK segment, which may

　　contain data

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/20170605143143.jpg)

　　**四次握手机制**

　　TCP连接管理：关闭

　　Step 1: client向server发送TCP FIN 控制segment

　　Step 2: server 收到FIN, 回复ACK. 关闭连接, 发送

　　FIN.

　　Step 3: client 收到FIN, 回复ACK.

　　• 进入“等待” –如果收到FIN，会重新发送ACK

　　Step 4: server收到ACK. 连接关闭.

![TCP协议图文秒懂](http://www.jizhuomi.com/upload/1234352-c4a54f9053ae9901.png)

　　由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。这原则是当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向的连接。收到一个 FIN只意味着这一方向上没有数据流动，一个TCP连接在收到一个FIN后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

　　（1） TCP客户端发送一个FIN，用来关闭客户到服务器的数据传送（报文段4）。

　　（2） 服务器收到这个FIN，它发回一个ACK，确认序号为收到的序号加1（报文段5）。和SYN一样，一个FIN将占用一个序号。

　　（3） 服务器关闭客户端的连接，发送一个FIN给客户端（报文段6）。

　　（4） 客户段发回ACK报文确认，并将确认序号设置为收到序号加1（报文段7）。

转载请标明本文地址：[http://www.jizhuomi.com/software/735.html](http://www.jizhuomi.com/software/735.html)

## 相关文章

- [TCP/IP协议攻击\-arp欺骗与icmp重定向](http://www.jizhuomi.com/software/581.html)  (2016\-5\-25 9:26:21)
- [TCP/IP之大明王朝邮差](http://www.jizhuomi.com/software/571.html)  (2016\-5\-16 9:39:5)
    
