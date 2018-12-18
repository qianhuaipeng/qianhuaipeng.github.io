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

#### 而且这些监控、调优工具的使用，无论你是运维、开发、测试，都是必须掌握的
#####  A、jps (Java Virtual Machine Process Status Tool)
> jsp主要用来输出JVM中运行的进程状态信息。语法格式如下：
```
jsp [option] [hostid]
```
 如果不指定hostid就默认当前主机或者服务器。
 命令行参数选项说明如下：
 ```
 -q 不输出类名、jar名和传入Main方法的参数
 -m 输出传入Main方法的参数
 -l 输出Main类或Jar的全限名
 -v 输出传入JVM的参数
 ```