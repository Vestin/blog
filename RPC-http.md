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
[PRC(Remote Procedure Call 远程方法调用)](https://en.wikipedia.org/wiki/Remote_procedure_call) 是**本地计算机程序**通过**网络**调用**远程计算机服务**。

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

## RPC-HTTP
HTTP 本质来讲是RPC调用的一种实现方式。换种方式说，**RPC客户端**可以通过HTTP连接到**RPC服务端**程序执行**RPC(远程方法调用)**

## RPC-REST
REST 是定义http接口调用的一种方式，REST 也可以说是RPC调用的实现方式。

* [phprpc解决方案](http://www.thinkphp.cn/extend/433.html)
* [hprose](http://hprose.com/)
* [phprpc office website](http://www.phprpc.org/zh_CN/)
* [yar(yet another RPC framework) github](https://github.com/laruence/yar)
* [为什么需要RPC，而不是简单的HTTP接口 (oschina)](http://www.oschina.net/question/271044_2155059?sort=default&p=1#answers)
* [RPC调用框架比较分析](http://www.tuicool.com/articles/jUj2miJ)