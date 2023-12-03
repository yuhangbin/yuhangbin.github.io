---
title: "Microservices in Action [0]"
date: 2023-06-17T22:40:20+08:00
author: "cboy"
tags: ["Microservice", "Java", "Programming"]
draft: false
---
# Introduction
该篇文章主要要用说明为什么要写这一系列的原因。在该系列中会按照搭建微服务的各个阶段所依赖的各个组件一个一个的去探索和分析碰到了什么问题，如何解决问题。
## 系列
- API Gateway
- CI & CD
- Service Discovery
- Communication between Services
- Configuration Center
- Observability
- Transaction & Distributed Lock Manager
#### 注意
本人报以60分的心态启动的该系列写作，理解有偏差或错误的请帮忙指出。后续会根据自己的认知的提升而进行改进文章。

# What

## 什么是Microservices?

Microservices是一种开发软件的架构和组织方法。通过把软件划分成多个独立的服务，服务之间通过明确的API来通信，每个服务拥有独立部署和迭代的能力。

## From Monolithic to Microservices

商业公司刚起步时，为了能快速迭代和上线功能，通常会采用单体应用（Monolithic）即应用的所有功能都在一个服务中进行开发和迭代。当公司的业务越来越多，单体应用的功能也会越来越多，开发人员也会越来越多，此时某个功能出现问题将影响到整体的可用性（单体应用随功能越来越多还会有其他的问题，此处只举一个关键的例子），而通常公司会采用微服务（microservices）的架构，把单体服务拆分成多个独立服务，服务间通过明确的API来通信，从而避免由于单点故障导致全局崩盘。尽管使用微服务架构可以避免该问题，但通过也带来了不少的“麻烦”，不同功能之间调用，原本只需要本地调用，现在需要依赖跨进程/跨机器调用（RPC/HTTP）并且需要考虑到调用失败的重试/容错处理；服务增多为了提升部署的效率和可靠性需要构建高效的CICD系统等等。权衡下来对于业务较复杂的大型商业公司来说，微服务是一种适用的解决方案。

# Why

## 写该系列的原因

以练促学：工作中较少有机会能完整的搭建微服务，通过实践提升对微服务的认知

# How

## 业务场景

UGC音乐平台（MusicHub）- 用户在该平台上即使创造者也是消费者，用户可以自由创作音乐，消费音乐。

仓库地址：https://github.com/yuhangbin/music-hub

### 歌曲服务(Song Service)

- 发布歌曲
- 获取歌曲信息（包含创作者信息）

### 歌单服务(PlayList Service)

用户可以通过歌单功能，实现自定义歌单

- 创建歌单
- 更新歌单
- 获取歌单（包含歌曲信息）

### 用户服务(User Service)

- 查询用户信息
- 注册
- 更新个人用户信息

## 系统设计
![microservices.png](/images/microservices.png)




# References
- Microservices Patterns With examples in Java (Chris Richardson)
- https://icyfenix.cn/
- https://en.wikipedia.org/wiki/Microservices
- https://aws.amazon.com/cn/microservices/
- https://microservices.io/
- https://en.wikipedia.org/wiki/Monolithic_application
- https://www.uber.com/en-JP/blog/microservice-architecture/
- https://microservices.io/patterns/apigateway.html
- https://github.com/ByteByteGoHq/system-design-101#microservice-best-practices