---
layout:     post  
title:      Spring Cloud微服务架构  
subtitle:    Spring Cloud微服务架构  
date:       2019-02-16  
author:     alan.peng  
header-img: img/spring/springcloud/logo.jpeg  
catalog: true  
tags:
    - Java  
    - Spring  
    - Spring Cloud  
---

### 1、什么是微服务(Microservice)
> 微服务就是将整个web应用按照业务和功能拆分成一系列小的web服务，这些微小的服务可以独立地进行编译和部署，并通过各自提供的API接口进行通讯。它们彼此相互协作，作为一个整体为用户提供服务，但是却可以独立的进行扩展。

#### 微服务需要的架构和使用的场景
- 我们把整个系统根据业务拆分成几个子系统
- 每个子系统可以集群部署，多个应用之间使用负载均衡
- 需要和一个服务注册中心，所有的服务都在注册中心注册，负载均衡也是通过在注册中心的服务来使用
- 所有的客户端都通过同一个网关地址访问后台服务，通过配置路由配置，网关来判断一个URL请求由哪个服务处理。请求装发到服务上的时候也是使用负载均衡
- 需要一个断路由，及时处理服务调用时的超时和报错，防止由于一个服务的问题而导致整个系统的瘫痪。（木桶原理）
- 还需要一个监控功能，监控每个服务调用花费的时间等。

目前主流的微服务框架： 阿里的Dubbo、Spring Cloud、Hessian

### 2、Spring Cloud 是什么鬼？
Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系统的开发。如服务发现与注册、配置中心、消息总线、负载均衡、断路由、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Sprig并没有重复造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉复杂的配置和实现原理，最终给开发者留出一套简单易懂、易部署、易维护的分布式系统开发工具包。

微服务是可以独立部署、水平扩展、独立访问的服务单元，Spring Cloud就是这些微服务的大管家。采用了微服务这种架构之后，项目的数量会非常多，Spring Cloud作为大管家需要管理好这些微服务，自然需要许多小弟来帮忙。

主要小弟有: Spring Cloud Config、Spring Cloud Netflix(Eureka、Hystrix、Zuul、Ribbon、Feign、Archaius...)、Spring Cloud Bus、Spring Cloud Security、Spring Cloud ...
### 核心成员
#### Spring Cloud Netflix
#### Netflix Eureka
服务中心，它提供一个服务注册中心、服务发现的客户端，还有一个方便的查看所有注册的服务界面。Spring Cloud 最牛鼻的小弟，任何小弟需要其它小弟支持什么都需要从这里获取，同样你有什么牛逼的功能都要过来注册，方便以后其它小弟过来调用；它的好处是你不需要直接找各种小弟的支持，只需要到服务中心来领取，也不需要知道提供支持的小弟在哪里，还是几个小弟在支持，反正拿来用就行，服务中心来保证稳定性和质量。
#### Netflix Hystrix
熔断器，容错管理工具，旨在通过熔断机制控制服务和第三方库的节点，从而对延迟和故障提供更强大的容错能力。比如我现在要调用A、B、C三个服务，调用A服务50ms，调用B服务50ms，但是调用C服务的时候发现C服务又依赖第三方的某个服务，由于第三方服务经常会出现超时现象导致我整个服务都延迟，这就是所谓的木桶定律，服务的性能全别这个C服务拉低了。这时候Hystrix就派上用场了，当Hystrix发现某个服务不稳定会马上让它下线，让其它小弟来顶上。
#### Netflix Zuul
网关，所有的客户端请求通过这个网关访问后台的服务。他可以使用一定的路由配置来判断某一个URL由哪个服务来处理。并从Eureka获取注册的服务来转发请求。（权限控制、验证token）
#### Netflix Ribbon
负载均衡，Zuul网关将一个请求发送给某个服务的时候，如果一个服务启动了多个实例，就会通过Ribbon来通过一定的负载策略来发送给某个服务实例
#### Netflix Feign
服务客户端，服务之间如果需要相互访问，可以使用RestTemplate，也可以使用Feign客户端访问。它默认会使用Ribbon来实现负载均衡

### Spring Cloud Config
配合中心，配置管理工具包让你可以把配置放到远程服务器，集中化管理集群配置，目前支持本地存储、Git和SVN。

### Spring Cloud Bus
事件、消息总线，用于在集群中传播状态变化，可以与Spring Cloud Config联合实现热部署。确保各个小弟之间的消息保持畅通。

### Spring Cloud Security


### 和Spring Boot是什么关系
Spring Boot 是Spring的一套快速配置脚手架，我们可以基于Spring Boot快速开发单个微服务，Spring Cloud是一个基于Spring Boot实现的云应用开发工具；Spring Boot专注于快速、方便集成的单个个体，Spring Cloud是关注全局的服务治理框架；Spring Boot使用默认大于配置的理念，很多集成方案已经帮你选好了，能不配置就不用配置，Spring Cloud是基于Spring Boot来实现。
> Spring > Spring Boot > Spring Cloud

### Spring Cloud 的优势
微服务包括dubbo、Kubernates等等
- 产于Spring大家族，Spring在企业级开发框架里面无人能敌，社区完善。dubbo由阿里团队开发
- 有Spring Boot 这个独立干将可以省很多事
- 作为一个微服务治理的大家伙，考虑很全面，几乎所有的服务治理的方方面面都考虑到了
- Spring Cloud社区活跃度很高，教程丰富
- 轻轻松松几行代码就能完成熔断、负载均衡、服务中心注册与发现等等功能




