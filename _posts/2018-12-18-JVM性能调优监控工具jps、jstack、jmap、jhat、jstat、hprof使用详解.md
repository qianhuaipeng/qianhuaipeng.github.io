---
layout:     post
title:      JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解
subtitle:   JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解
date:       2018-12-18
author:     alan.peng
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - java
    - jvm
---

现实企业级Java应用开发、维护中，有时候我们会碰到下面这些问题：
- OutOfMemoryError，内存不足
- 内存泄露
- 线程死锁
- 锁争用（Lock Contention）
- Java进程消耗CPU过高
- ...
 
 这些问题在日常开发、维护中可能会被很多人忽视（比如有的人遇到上面的问题只是重启服务器或者调大内存，而不会深究问题的根源），但能够理解并解决这些问题是java程序员进阶的必备要求。本文将对一些常用的JVM性能调优监控工具进行介绍，希望能抛砖引玉之用。

### 而且这些监控、调优工具的使用，无论你是运维、开发、测试，都是必须掌握的
####  A、jps (Java Virtual Machine Process Status Tool)
> jsp主要用来输出JVM中运行的进程状态信息。语法格式如下：

    jsp [option] [hostid]

 如果不指定hostid就默认当前主机或者服务器。
 命令行参数选项说明如下：

     -q 不输出类名、jar名和传入Main方法的参数
     -m 输出传入Main方法的参数
     -l 输出Main类或Jar的全限名
     -v 输出传入JVM的参数

比如下面：
    
    root@ubuntu:/# jps -m -l
    2458 org.artifactory.standalone.main.Main /usr/local/artifactory-2.2.5/etc/jetty.xml
    29920 com.sun.tools.hat.Main -port 9998 /tmp/dump.dat
    3149 org.apache.catalina.startup.Bootstrap start
    30972 sun.tools.jps.Jps -m -l
    8247 org.apache.catalina.startup.Bootstrap start
    25687 com.sun.tools.hat.Main -port 9999 dump.dat
    21711 mrf-center.jar

#### B、jstack
 jstack主要用来查看某个Java进程内的线程堆栈信息。语法格式如下：

    jstack [option] pid
    jstack [option] executable core
    jstack [option] [server-id@] remote-hostname-or-ip

命令行参数选项说明如下：
                    
    -l long listings，会打印出额外的锁信息，在发生死锁时可以用jstack -l pid 来观察锁持有情况 -m mixed mode，不仅会输出Java堆栈信息，还会抛出C/C++堆栈信息（比如native方法）

