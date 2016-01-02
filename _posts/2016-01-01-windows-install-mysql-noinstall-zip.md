---
layout: post
title: 在 Windows 上使用 noinstall Zip 文件安装 MySQL
date: 2016-01-01 01:08
author: admin
comments: true
categories: [MySQL]
tags: [MySQL,安装]
---

在 Windows 环境下，最简单的安装 MySQL 方式自然是使用  MySQL Installer 进行“傻瓜”式点下一步安装即可，可以参见[mysql-5.6.13在windows平台下的安装、使用（图解）](http://www.waylau.com/mysql-5-6-13-windows-platform-installation-use-graphic/)。本文介绍了另外一种使用 noinstall Zip 文件（免安装版）安装 MySQL。

<!-- more -->
## 下载安装包

地址： <http://dev.mysql.com/downloads/mysql/>

本例使用 MySQL 版本为：Windows (x86, 64-bit), ZIP Archive	5.7.10	(mysql-5.7.10-winx64.zip)

本例使用的操作系统为：Win7 Sp1 x64

## 解压到安装目录

1. 确保使用管理员权限的用户登陆操作系统
2. 选择安装位置。传统上，MySQL server 安装在 `C:\mysql`。 MySQL 的安装向导（MySQL Installer）安装 MySQL 在`C:\Program Files\MySQL`。如果您没有在 `C:\mysql` 安装 MySQL，你必须在启动或者在  option file 文件中指定的安装目录路径。本例安装在`C:\Program Files\MySQL`目录。
3. 将安装包解压后放到安装目录下，如下：

![](http://a.picphotos.baidu.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=f40f5365d188d43ff4a991f74d25a326/9345d688d43f8794accebc3ed51b0ef41bd53ae8.jpg?referer=a368c26f7ff0f73681e9793186a6&x=.jpg)


## 创建配置文件

配置文件是用于设置 MySQL 的配置，解压包下有一个 my-default.ini 文件，就是模版。复制该模版从命名为 my.ini，则 MySQL 就会从 my.ini 里读取配置。

my.ini 放在 `C:\Windows`目录。

本例安装目录和数据设置在 (`C:\Program Files\MySQL\mysql-5.7.10-winx64` 和 `D:\mysqlData\data`)，配置如下：

```
[mysqld]
# set basedir to your installation path
basedir = C:/Program Files/MySQL/mysql-5.7.10-winx64
# set datadir to the location of your data directory
datadir = D:/mysqlData/data
```

MySQL 5.7.6 之后，noinstall 安装包并没有提供 data 目录，所以要初始化一个 data 目录

## 添加 MySQL 工具到 PATH

为了方便使用  MySQL 工具，添加 MySQL bin 目录添加到 PATH 是个不错的选择。

本例将 `C:\Program Files\MySQL\mysql-5.7.10-winx64\bin`添加到 PATH,如下：

![](http://a.picphotos.baidu.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=92f21ca8d33f8794d7ff482be2207fc9/d788d43f8794a4c27da9183b09f41bd5ad6e3913.jpg?referer=03f8b2d0f036afc3571b0b5587f1&x=.jpg)

## 初始化 data 目录

使用命令行

```
mysqld --defaults-file=C:\Windows\my.ini --initialize
```

其中 `--defaults-file` 指向了我们之前创建的 my.ini 文件

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

ads

```ruby
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
```

## 选择 MySQL server 类型
## 初始化 MySQL
## 启动  MySQL server
## 加密默认用户账号

## 问题

```
C:\Users\Administrator>mysqld --defaults-file=C:\Windows\my.ini --initialize
mysqld: Can't create directory 'D:\mysqlData\data\' (Errcode: 2 - No such file o
r directory)
2016-01-01T15:08:10.531737Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is
 deprecated. Please use --explicit_defaults_for_timestamp server option (see doc
umentation for more details).
2016-01-01T15:08:10.531737Z 0 [Warning] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'E
RROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will
be merged with strict mode in a future release.
2016-01-01T15:08:10.531737Z 0 [Warning] 'NO_AUTO_CREATE_USER' sql mode was not s
et.
2016-01-01T15:08:10.534737Z 0 [ERROR] Aborting
```

时间（时区）冲突


## 参考

* <http://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html>
* <http://www.waylau.com/mysql-5-6-13-windows-platform-installation-use-graphic/>
* <http://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization-mysqld.html>