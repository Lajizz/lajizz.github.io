---
title: the Outline of Transform Layer in Network
tags: ComputerNetwork
---

# 运输层

## 概述与运输层服务
* 工作在网络层与应用层之间，只为上层提供必要的服务，如可靠的数据传输等
### 运输层和网络层
* 网络层为不同主机提供逻辑通信，传输层为不同主机之间的进程通信
* 运输层工作在网络层之上，而通常网络层提供的是`不可靠`的运输服务，但运输层常常要求是`可靠的`,这就涉及到各种协议设计机制
* 运输层与网络层的多路复用与多路分解，这涉及到`port`
* 运输层是要考虑，可靠性(`TCP与UDP`,rdt1-rdt3)，流量控制(流量窗口,rdt3.1)，公平性(`多个TCP连接`)，安全性(`加密校验等,也可包含在可靠性之中`)，拥塞控制(`AIMD 加性增，乘性减`)等多个问题

## 重要的传输层协议TCP与UDP
### 特点不过多赘述，注意以下几点
* 套接字与多路复用、多路分解的关系
* 周知端口号
* 套接字二元组(ip,port)
* web服务器会为每个到80端口的TCP分配一个新的进程
* DNS使用UDP
* UDP有差错检查并没有差错回复，并且有可能和TCP产生竞争

## 构造可靠的数据传输协议
> 其原理不仅在传输层协议设计有用，同样适用于其他层协议设计

### rdt1.0
* 信道完全可靠
### rdt2.0
* 具有比特差错信道
* 增加了ASK、NAK与差错检测,重传

### rdt2.1 
* 考虑到ASK,NAK丢失
* 添加序号

### rdt3.0 
* 会发生比特差错，并且会丢包的信道
* 夹定时器

#### 具有检验和，序号，定时器，肯定和否定确认的协议已经是一个__传输可靠__的协议，也仅限于可靠

## 流水线可靠数据传输协议:解决rdt3.0的性能问题
### 三种ARQ方案
* 经停协议
* Go-Back-N
* Selective-Repeat SR

## 回到TCP与UDP

### TCP
#### 三次握手-cookie的制作与防止synflood
#### 连接的解除
#### 报文结构
#### 累计确认：会把先到的存在缓冲区，等中间的到了一起确认
#### RTT:往返时间
* sampleRTT:RTT在某一时刻
* EstimatedRTT：许多SampleRTT的平均，需要维持到一个具体的值
 * 参考计算公式: $EstimatedRTT_{new}\;=\;0.875\;\times\;EstimatedRTT_{old}\;+\;SampleRTT_{new}$
* DevRTT: 偏差RTT
* $TimeoutInterval\;=\;EstimatedRTT+\;4\times DevRTT$
 * TimeoutInterval 一般设置为1s

#### 超时间隔加倍：TimeoutInterval
#### 快速重传：收到三个冗余ASK
#### GBN或SR
#### 流量控制（注意与拥塞机制的区别）
* $rwnd\;=\;RcvBuffer\;-\;\lbrack\;LastByteRcvd\;-\;LastByteRead\rbrack$
* $rwnd\;\geq\;LastByteSent\;-\;LastByteAsked$

#### 拥塞控制
##### 两种方法
1. 端到端拥塞控制：传输层本身感知
2. 网络辅助的拥塞控制： 通过路由器等
* ATM ABR 特殊信元
#### TCP拥塞控制：端到端拥塞控制
* 通过维护拥塞窗口cwnd
* 自计时
 * 丢包意味拥塞
 * ASK表示正常交付
 * 通过ASK和丢包事件进行带宽检测
* TCP拥塞控制算法
 1. 慢启动：指数（以下都是针对cwnd而言）
 2. 拥塞避免：线性增 ，乘性减
 3. 快速恢复：不回到0
#### 吞吐量
* $average\;=\;\frac{0.75\;\times W}{RTT}&
> W 某一特定时刻cwnd窗口字节长度

* 高带宽吞吐量
 * $average\;=\;\frac{1.22\;\times MSS}{RTT\sqrt L}$
 > L:丢包率 MSS:一个较小值

#### 公平性：公平分配带宽 R/K
* 两条TCP连接 在传输速录为R的瓶颈路段的收敛结果
* 多条TCP连接
* TCP连接与TCP连接

