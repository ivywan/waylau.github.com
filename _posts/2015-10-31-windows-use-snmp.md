---
layout: post
title: Windows 下使用 SNMP 来管理网络
date: 2015-10-31 01:08
author: admin
comments: true
categories: [SNMP]
tags: [SNMP,Windows]
---

本文介绍了 SNMP 协议 的基本概念，SNMP 的作用，以及如何在 Windows 下使用 SNMP 来管理网络中的设备。

<!-- more -->

## SNMP 的介绍

SNMP（Simple Network Management Protocol，简单网络管理协议）是 Internet 协议簇的一部分。该协议能够支持网络管理系统，用以监测连接到网络上的设备是否有任何引起管理上关注的情况。它由一组网络管理的标准组成，包含一个应用层协议（application layer protocol）、数据库模型（database schema），和一组数据对象。

利用SNMP，一个管理工作站可以远程管理所有支持这种协议的网络设备，包括监视网络状态、修改网络设备配置、接收网络事件警告等。

## 在 Windows 下使用 SNMP

### 确保已经安装snmp组件

在 Windows组件—>管理和监视工具，勾选“WMI SNMP 提供程序”。

![](http://99btgc01.info/uploads/2015/10/000.png)

### 设置 SNMP Service

添加一个社区名称 “public”,权限设为“只读”或者“读写”

![](http://99btgc01.info/uploads/2015/10/001.png)

添加可以接受 SNMP 的主机地址，一般就是远程主机的 IP,如果是提供给本地读的，就填“localhost”即可

![](http://99btgc01.info/uploads/2015/10/002.png)


## 参考

* 源码见：<https://github.com/waylau/snmp4j-demos>
* <https://en.wikipedia.org/wiki/Simple_Network_Management_Protocol>