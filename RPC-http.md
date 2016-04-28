---
title: RPC-http
toc: true
categories:
  - coding
tags:
  - RPC
  - http
  - php
date: 2016-04-19 11:10:42
description:
---
[TOC]

## RPC是什么
[PRC(Remote Procedure Call 远程过程调用)](https://en.wikipedia.org/wiki/Remote_procedure_call) 是**本地计算机程序**通过**网络**调用**远程计算机服务**。

<!--more-->

## 为什么要用RPC
1. 可以做到分布式，现代化的微服务
2. 部署灵活
3. **解耦服务**
4. 扩展性强

RPC的目的是让你在本地调用远程的方法，而对你来说这个调用是透明的，你并不知道这个调用的方法是部署哪里。通过RPC能解耦服务，这才是使用RPC的真正目的。

## RPC结构
client-server 结构，调用方为client，远程被调用方为server。

## RPC工作原理
1.调用客户端句柄；执行传送参数
2.调用本地系统内核发送网络消息
3.消息传送到远程主机
4.服务器句柄得到消息并取得参数
5.执行远程过程
6.执行的过程将结果返回服务器句柄
7.服务器句柄返回结果，调用远程系统内核
8.消息传回本地主机
9.客户句柄由内核接收消息
10.客户接收句柄返回的数据

## RPC框架有哪些
一般主流框架都实现了跨平台跨语言的C/S RPC调用。
* [`dubbo`](http://dubbo.io/),主流配合hessian协议使用,duboo/hessian.
>DUBBO是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，是阿里巴巴SOA服务化治理方案的核心框架，每天为2,000+个服务提供3,000,000,000+次访问量支持，并被广泛应用于阿里巴巴集团的各成员站点。
* [`thrift`](https://thrift.apache.org/),Apache Thrift software framework
>The Apache Thrift software framework, for scalable cross-language services development, combines a software stack with a code generation engine to build services that work efficiently and seamlessly between C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, OCaml and Delphi and other languages.
* [`hprose`](http://hprose.com/),High Performance Remote Object Service Engine
>是一款先进的轻量级、跨语言、跨平台、无侵入式、高性能动态远程对象调用引擎库。它不仅简单易用，而且功能强大。
你无需专门学习，只需看上几眼，就能用它轻松构建分布式应用系统。

## RPC-HTTP
HTTP 本质来讲是RPC调用的一种实现方式。换种方式说，**RPC客户端**可以通过HTTP连接到**RPC服务端**程序执行**RPC(远程过程调用)**。

把RPC比作交通工具，那么HTTP就是相当于汽车

#### HTTP 调用优点
* 协议统一，各个平台几乎都原生支持HTTP
* 调用简单,直接
* 开发方便

> HTTP（HyperText Transfer Protocol）是应用层通信协议, HTTP 的缺点是协议头较重，一般请求到具体服务器的链路较长，可能会有 DNS 解析、Nginx 代理等。

#### RPC 框架的优点
* RPC框架一般使用长链接，不必每次通信都要3次握手，减少网络开销
* RPC框架一般都有注册中心，有丰富的监控管理
* 发布、下线接口、动态扩展等，对调用方来说是无感知、统一化的操作
* 协议私密，安全性较高
* rpc 协议更简单内容更小，效率更高
* 服务化架构、服务化治理，RPC框架是一个强力的支撑

## RPC-REST
REST 是定义http接口调用的一种方式，REST 也可以说是RPC调用的实现方式。

* [phprpc解决方案](http://www.thinkphp.cn/extend/433.html)
* [hprose](http://hprose.com/)
* [phprpc office website](http://www.phprpc.org/zh_CN/)
* [yar(yet another RPC framework) github](https://github.com/laruence/yar)
* [为什么需要RPC，而不是简单的HTTP接口 (oschina)](http://www.oschina.net/question/271044_2155059?sort=default&p=1#answers)
* [RPC调用框架比较分析](http://www.tuicool.com/articles/jUj2miJ)
* [RPC框架几行代码就够了](http://javatar.iteye.com/blog/1123915)
* [支撑微博千亿调用的轻量级RPC框架：Motan](http://h2ex.com/820)
