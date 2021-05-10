---
title: 套接字 socket
date: 2021-03-06 13:57:04
tags:
---

## socket 分类

### 流 stream

流 socket 基于 TCP 协议，面向连接的可靠服务。保证数据按序到达

### 数据报 datagram

数据报 socket 基于 UDP 协议，无连接的不可靠服务。数据可能会丢失，不保证按序到达，且数据包长度有限制，但传输效率高（无连接、无确认）
