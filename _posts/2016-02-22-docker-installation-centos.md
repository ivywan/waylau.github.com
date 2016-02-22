---
layout: post
title: Docker 在 CentOS 下的安装、使用
date: 2016-02-22 02:41
author: admin
comments: true
categories: [Docker]
tags: [Docker,CentOS,安装,容器]
---
本文介绍了 Docker 在 CentOS 环境下的安装、使用。

<!-- more -->

## 什么是 Docker

[Docker](http://www.docker.com/) 是[开源](https://github.com/docker/docker)的应用容器引擎。

Docker 可以让你将所有应用软件以及它的以来打包成软件开发的标准化单元。

Docker 容器将软件以及它运行安装所需的一切文件（代码、运行时、系统工具、系统库）打包到一起，这就保证了不管是在什么样的运行环境，总是能以相同的方式运行。

![](https://docs.docker.com/dist/assets/images/logo.png)

更多有关 Docker 的介绍，可以参阅《[简述 Docker](http://waylau.com/ahout-docker/)》 一文。

## 前置条件

* 64-bit 系统
* kernel 3.10+
* CentOS 7+。本例使用的是`CentOS-7-x86_64-Minimal-1511.iso`

使用 `uname -r` 检查 kernel 版本

    $ uname -r
    3.10.0-327.el7.x86_64

建议你使用最新的系统，以为一直的 bug 都会在新的 kernel 发布中修复。

## 安装

### yum 方式安装

1.使用 sudo 或 root 权限的用户登入系统。

2.确保你的 yum  是最新的

    sudo yum update
    
3.添加 yum 仓库

    $ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
    [dockerrepo]
    name=Docker Repository
    baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
    enabled=1
    gpgcheck=1
    gpgkey=https://yum.dockerproject.org/gpg
    EOF

4.安装 Docker 包


## 创建 docker group

## 启动 docker 守护进程

## 卸载

## 参考

