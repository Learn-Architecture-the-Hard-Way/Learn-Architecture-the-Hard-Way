# Learn Architecture the Hard Way

![GitHub](https://img.shields.io/github/license/Learn-Architecture-the-Hard-Way/Learn-Architecture-the-Hard-Way)
[![Github](https://img.shields.io/badge/orginzation_project-Learn_Architecture_the_Hard_Way-brightgreen)](https://github.com/Learn-Architecture-the-Hard-Way)
[![Github](https://img.shields.io/badge/author-Jover_Zhang-brightgreen)](https://www.joverzhang.com)

# Introduction

### If you have any questions while reading?

You can:

- Use the search engine.
- Submit an Issue.
- Contact the author. < joverzh@gmail.com >

# What is Architecture?

## 0. Preface

本文希望以简化地方式阐述架构发展中的服务拆分过程，并不涉及分布式架构带来的数据一致性、数据库性能瓶颈等问题，希望有兴趣的读者可以通过自行了解：

- 分布式缓存
- 分布式锁
- 数据库水平/垂直切分
- 分布式事务

Let's go !

## 1. Simple Servers

目前，仅需要**单**个服务器即可满足性能需求。

```
           +-----------+
client --> | service A |
           +-----------+
```

## 2. Load Balance

当单个服务器无法满足性能需求时，开始采取**水平扩展**并引入**负载均衡**技术。

```
                          +-----------+
                        / | service A |
           +---------+ /  +-----------+
client --> | gateway |  
           +---------+ \  +-----------+
                        \ | service A |
                          +-----------+
                                .
                                .
                                .
```

## 3. Distributed Architecture

当业务达到一定量级后，开发人员需要维护的代码开始变得极为庞大和复杂，难免出现业务代码迭代时牵一发而动全身。  
因此出现**服务拆分**，从而降低开发人员的开发难度，并使得服务部署更加灵活，各个服务部署的数量可根据业务访问量自行调整。  

```
                          +-----------+
                        / | service A |----------+
           +---------+ /  +----------++service A | ...
client --> | gateway |               +-----------+
           +---------+ \  +-----------+
                        \ | service B |----------+
                          +----------++service B | ...
                                     +-----------+
                                .
                                .
                                .
```

## 4. Microservices

从实践的角度来看，**微服务架构通常也是分布式架构**，反之则未必成立，分布式架构已经可以解决了目前的大部分问题，**而微服务则更强调服务的专业化和精细分工**。  
在分布式架构中，经过业务的迭代，难免分布式服务会出现**功能重复**的现象。  
而**微服务**思想则是把复用的功能抽离成微服务。

**Core层**是以微服务方式实现核心功能，再由**Composite层**以实现业务需求为目的调用各个**Core层**的微服务。

**Core层**的微服务允许相互调用其它**Core层**的微服务，而**Composite层**一般直接面向客户端，复用率较低，故一般在**Composite层**会明令禁止相互调用，以免产生高耦合。

```
                          Composite Microservices               Core Microservices

                          +-----------+                      +-----------+
                        / | service A |----------+           | service C |----------+
           +---------+ /  +----------++service A |  \     /  +----------++service C | ...
client --> | gateway |               +-----------+   \  /               +-----------+
           +---------+ \  +-----------+               X      +-----------+
                        \ | service B |----------+  /   \    | service D |----------+
                          +----------++service B | /      \  +----------++service D | ...
                                     +-----------+ \                    +-----------+
                                .          .        \        +-----------+
                                .          .         \       | service E |----------+
                                .          .          \      +----------++service E | ...
                                           \\          \                +-----------+
                                            \\          \    +-----------+
                                             \\          \-> | service F |----------+
                                              \\             +----------++service F | ...
                                               \\                       +-----------+
                                                \\                 .
                                                 \==============>  .
                                                                   .
```

# About Spring Cloud

## Preface

以上的理论知识告一段落，从此处将开启**实现**部分。

**About Spring Cloud**的主要目的是为了使得每一个位架构爱好者(即使你并不是专业的开发人员)，都可以轻松部署起来一套完整的微服务环境。  
内容分为多章节，以业务需求为导向循序渐进，让读者能够更轻松地理解微服务中各种*不知道有什么用*的组件**出现的原因**。

## [Spring Cloud Netflix](https://github.com/Learn-Architecture-the-Hard-Way/Spring-Cloud-Netflix)

此项目主要是为了使得更多的人能够快速地了解**Spring Cloud Netflix**的主要功能及其**存在的意义**。  
并以现实业务需求为导向，以微服务架构为核心，使用多个案例**循序推进**架构的样貌。

但也因微服务架构涉及多个服务，若每个独立部署，较为繁琐且容易出错。该项目为使读者能够更轻易地在自己本地部署微服务环境，引用了**Docker**与**Docker Compose**作为部署工具。
当你在你的计算机安装了**Docker**与**Docker Compose**之后( [下方有安装链接](https://github.com/Learn-Architecture-the-Hard-Way/Spring-Cloud-Netflix#technology-used) )，
你最多只需要执行**两行指令即可运行**一个案例。

在**Docker Compose**的控制台信息，可清晰看到负载均衡与链路调用的真实情景。

[点击此处跳转到项目仓库...](https://github.com/Learn-Architecture-the-Hard-Way/Spring-Cloud-Netflix)

## [Spring Cloud Alibaba](#)

**Next...**
