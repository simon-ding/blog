---
title: "微服务介绍以及 go 语言微服务实践"
date: 2020-01-17T22:19:48+08:00
lastmod: 2020-01-17T22:19:48+08:00
draft: false
keywords: []
description: ""
tags: ["编程"]
categories: ["编程"]
author: "simon"
---

这几年微服务的概念大火。很多做开发的都听说过微服务这个概念，最近我也在自己负责的项目当中实践了一次微服务。

## 什么是微服务

知乎上有个回答，对这个问题回答的挺好：[什么是微服务架构？ - 老刘的回答 - 知乎](https://www.zhihu.com/question/65502802/answer/802678798)

我在这里简单说一下。一般来说开发软件的初期都会选择单体架构(Monolithic Architecture)，所有功能都打包到一个单一程序当中。

随着项目的迭代，这个单体应用的功能会越加越多，例如一个电商应用可能包含了用户管理、订单管理、商品管理、和促销活动等不同功能的模块，现在是移动互联网的时代，不仅要有web界面的api，移动端可能也要单独维护一套api。

这样这套架构也变得非常复杂，各个功能之间的耦合也非常严重。不利于新功能开发，而且一旦熟悉系统的老员工离职，新来的员工也非常难以接手这套系统。

微服务构架就是对这个挑战的一种解决方案之一。简单来说，微服务就是把原来大而全的单体应用拆分成功能独立的、自洽的小而美的多个应用的组合。

理想的微服务架构下各个服务可以用用最擅长的语言来写。例如，web业务方面可以用java来写，数据处理方面的功能可以用python来写，高性能计算任务可以用C++来写。这样，各个语言可以做自己最擅长的事情。

然而理想是丰满的，而现实是骨感的。现阶段不管是java系的spring cloud或dubbo还是go语言的go micro和go-kit都限制使用单一语言来开发。（下一阶段的service mesh框架可以解决这个问题，这不在本次的讨论之中）

## 框架选型

微服务带来很多好处的同时，也增加了很多复杂度。微服务架构本质上来说是一种分布式框架，要解决很多由于分布式带来的问题：服务注册、服务发现、配置管理、链路追踪……

我们可以自己解决这个问题，但为了方便起见我们还是选择成熟的框架，让框架来处理这些细节问题，开发人员更专注于功能的开发。

由于我们项目使用的go语言，调研了几个go语言的微服务框架：

### go-kit

[go-kit](https://github.com/go-kit/kit) 是用来构建go微服务（或优雅单体应用）的工具包。

go-kit更关注于集成微服务的各种工具，本身没有提供太多抽象。其项目更想融入你现有的系统当中去，而不是提供一个大而全的解决方案

### go micro

[go-micro](https://github.com/micro/go-micro) 是go语言微服务开发框架。

go-micro引入了更多的功能，提供了微服务的一站式解决方案：

 * 服务发现
 * 负载均衡
 * 消息编码
 * 请求/响应
 * 异步消息
 * 可插拔接口

go-micro还提供了一个简单的API网关，也叫micro。可以用作反向代理和负载均衡等功能。

### 其它框架

B站也有其开源的 [Kratos](https://github.com/bilibili/kratos)，另外华为貌似也有个servercomb框架，我没有做过多研究。

## 架构

根据我们的需求，选择go-micro这种比较全面的框架更适合我们。我们系统的微服务框架如下：
![tea-arch](../../../../img/tea-arch.png)

* UI作为用户的入口，用户通过浏览器访问web页面
* micro作为api网关，反向代理tea-api部分的rest api，同时提供负载均衡的功能。
* etcd作为服务注册和服务发现中心，同时充当配置中心的功能

### 服务拆分
搭好基本的框架之后我们就要拆分现有的服务。我们的项目算是一个NLP平台，根据定义的规则，解析输入的语句，如果语句符合对应的规则，就打上对应的标签。

所以对于我们这个项目核心功能就是规则的解析和语句的匹配功能，我把这块叫做tea-core。另外有一部分负责和web交互，提供对应的api。另外还提供了tea-core之上封装的一些功能：规则树编辑、跑批支持、测试集、系统配置等等功能，我把这块叫做tea-web。

在代码中这两块本来耦合就比较少，拆分过程还是很容易的。拆分出来的两部分通过grpc通讯，解决两部分耦合的问题。


## 部署
项目代码推送到主分支之后，我们会使用gitlab的CICD工具，自动化构建docker镜像。然后推送到我们私有的阿里云docker registry服务器上去。部署时使用docker-compose来部署
。

由于自动化CICD的应用和docker镜像的打包，极大的减轻了我们自己的运维负担。

## 感悟

自己之前也没有接触过微服务，这算是自己做的第一个微服务实践。由于公司也没有懂这方面的人，一切都是我自己搜集文档、了解微服务当中的概念，一点点试错，一点点搭起来的。但第一次实践微服务，肯定有很多不成熟的地方。  

