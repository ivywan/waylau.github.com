 
[memcached-session-manager](https://github.com/magro/memcached-session-manager) 是基于 [Memcached](http://memcached.org/) 高可用性 Tomcat 集群开源解决方案。主要用于管理、解决分布式 Tomcat 环境下的 session 共享的问题。

memcached-session-manager 除了可以从本机内存快速读取 session 信息(仅针对 sticky session)外，同时可使用 memcached 存取 session，以实现高可用。
对于 non-sticky session，memcached 直接存储 session。
除 memcached 外，还可以兼容其他缓存组件如 [memcachedb](http://memcachedb.org/), [membase](http://membase.org/) 等。

## 特性
 	 
* 支持 Tomcat6、Tomcat7、Tomcat8
* 支持 sticky session 或 non-sticky session
* 无单一故障点
* 可处理 Tomcat 故障转移
* 可处理 Memcached 故障转移
* 插件式 session 序列化
* 允许异步保存 session，以提升响应速度
* 只有当 session 有修改时，才会将 session 写回 Memcached
* JMX 管理&监控

*sticky session 与 non-sticky session 区别*

*在“负载均衡”中，sticky session 模式下，会将同个 session 或者同个 IP 地址的请求，路由到同一个 Web 服务器进行处理。而 non-sticky session 模式则刚好相反，同个客户端的多次请求有可能被路由到不同的 Web 服务器进行处理。*


## 解决什么问题

假设你有一个 Tomcat 集群，使用 sticky session，如何应对 session 故障问题？为了应对更多的并发量和可用性，你可以不断的增加 Tomcat 节点，但是单点故障仍旧会是个问题。

解决此问题的思路就是将 session 同时保存在 Memcached 中，如果单个Tomcat 发生故障，集群中的其他 Tomcat 可以从 Memcached 中得到session 信息。

自 1.4.0 版本起，支持 non-sticky session，在并发请求中可选锁定 session ，同时避免单点故障（session 存储在用于备份的二次Memcached 中）。

## 工作机制

### 对于 sticky session

安装在 Tomcat 上的 memcached-session-manager 使用本机内存保存 session，和 [StandardManager](http://tomcat.apache.org/tomcat-6.0-doc/api/org/apache/catalina/session/StandardManager.html) 一样。

另外，当一个请求结束时，session（如果存在的话）会被送回 Memcached 节点进行备份。

当下一次请求开始时，本地 session 可用，直接服务，请求结束后，session 又被送回 Memcached 备份。

当集群中的一个 Tomcat 挂掉，下一次请求会被路由到其他 Tomcat 上。负责处理此此请求的 Tomcat 并不清楚 session 的信息。此时它会从Memcached 节点查找该 session，更新该 session 并将其保存在本机 jvm 中。自此之后，该 Tomcat 就负责这个 session 的服务。此次请求结束，session 被修改，送回 Memcached 备份。这就是 Tomcat 故障转移的完整处理过程。

![](http://dl.iteye.com/upload/picture/pic/115438/dfc721d4-1d0a-34ad-b5dc-304d9a82fdce.jpeg)


上边介绍的是处理 Tomcat 故障转移，如何处理 Memcached 故障转移呢？

如果一个 Memcached 节点故障，当前 Memcached 中的 session 会转移到其他 Memcached 节点，同时，JSESSIONID 被修改并送回浏览器。
如果使用 sticky session，应确保负载均衡器中配置生成的是“普通”的 sessionId，而不加任何其他后缀。

memcached-session-manager 知道 Memcached 节点列表(比如n1.localhost:11211 n2.localhost:11212 ，其中 n1 为 Memcached 节点标识)，这些节点标识会存储在 sessionId 中，sessionId 值类似：`602F7397FBE4D9932E59A9D0E52FE178-n1` 。


## 快速入门示例


### 下载、安装 Memcached 

先安装 Memcached 依赖 libevent

* Debian/Ubuntu: apt-get install libevent-dev 
* Redhat/Centos: yum install libevent-devel
* SUSE/openSUSE: zypper install libevent-devel


下载 Memcached 源码:

    wget http://memcached.org/latest

下载后的包名为`latest`,也可以将其改名为`memcached-1.4.25.tar.gz`，解压后生成文件夹为`memcached-1.4.25`

解压、编译、安装 

    tar -zxvf memcached-1.4.25.tar.gz
    cd memcached-1.4.25
    ./configure && make && make test && sudo make install

安装完成后会把memcached放到 `/usr/local/bin/memcached` ，

测试安装是否完成

    waylau:/home/software/memcached-1.4.25 #  ls -al /usr/local/bin/mem*
    -rwxr-xr-x 1 root root 444176 May  8 17:18 /usr/local/bin/memcached

启动 

    memcached  -p 11211 -m 64m -d -u root

其中 memcached 启动选项如下：

选项	 | 说明
---- | ----
-p	 | 使用的TCP端口，默认为 11211
-m	 | 最大内存大小默认为 64m
-v | 调试输出error/warning
-vv	| 用 very verbose 模式启动，调试信息和错误输出到控制台
-vvv | 调试输出内部状态
-d	 | 作为 daemon 在后台启动
-c | 最大并发连接数
-l | 监听的服务器IP地址
 

客户端访问 Memcached

    telnet localhost 11211  
    Trying ::1...
    <36 new auto-negotiating client connection
    Connected to localhost.
    Escape character is '^]'.
    
连接到 memcached 之后，输入`stats`再按回车，即可获得包括资源利用率在内的各种信息；此外，输入`stats slabs`或`stats items`还可以获得关于缓存记录的信息。结束程序输入`quit`


关于`set <key> <flags> <exptime> <bytes> \r\n <value> \r\n` 命令
* key: key就是你要存储数据的键值, 数据通过key来获取
* flags: 无符号的32位整型,客户端提供的参数,用于标识数据格式,比如使用MEMCACHE_COMPRESSED表示数据压缩存储.还有json, xml等.对服务端而言,这个参数并不知道是做什么用的.
* exptime: 写入缓存失效的时间, 单位是秒, 0表示数据永久不过期, 除非当前内存不够用了使用LRU算法来回收该段内存.
* bytes: 用来缓存value的数据块的字节数, <value>的大小不能超过此<bytes>
* \r\n: 表示回车换行
* value: 表示要缓存的数据


```
set foo 0 0 10
<36 set foo 0 0 10
waylau.com
>36 STORED
STORED
get foo
<36 get foo
>36 sending key foo
>36 END
VALUE foo 0 10
waylau.com
END

```

## 参考引用

* <https://github.com/magro/memcached-session-manager/wiki>
* <http://wiki.metawerx.net/wiki/StickySessions>
* <http://www.iteye.com/topic/1125301>
* <https://github.com/lzjun567/note/blob/master/note/memcached/telnet_command.md>
