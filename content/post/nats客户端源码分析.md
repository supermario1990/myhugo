---
title: "Nats客户端源码分析"
date: 2020-09-23T14:44:07+08:00
draft: true
comments: true
categories:
- golang
- nats
tags:
- golang
- nats
- 消息队列
---

[nats.go](https://github.com/nats-io/nats.go) 是消息队列 [nats-server](https://github.com/nats-io/nats-server) golang客户端，本文先对客户端的实现进行源码分析，后续对 nats-server 进行分析。

消息队列最基本的通信模式就是【订阅】- 【发布】. nats 基于【订阅】- 【发布】模式还实现了【请求】-【应答】模式

{{< imgcap title="请求-应答模式" src="../../images/nats-req-rply.png" >}}





