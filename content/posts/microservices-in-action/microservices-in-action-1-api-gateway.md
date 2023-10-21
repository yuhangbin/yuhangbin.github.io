---
title: "Microservices in Action 1 - Api Gateway"
date: 2023-07-02T20:54:35+08:00
author: "cboy"
tags: ["Microservice", "Programming"]
draft: true
---
# What & Why
## What is an API Gateway?
我们先对API Gateway进行结构，很直观的可以看出该词有两个词语组成 API 和 Gateway。我们分别探索一下API和Gateway分别是什么？然后再去探索API Gateway。
### What is API?
> An application programming interface(API) is a way for two or more computer programs to communicate with each other. It is a type of software interface, offering a service to other pieces of software[1].
API是一种应用编程接口，是一种提供两个计算机程序通信的方式。API是一种软件接口，提供服务供其他软件调用。
### What is Gateway?
> 
### API Gateway

// explain Gateway
    // layer 4 and layer 7
// Load Balancer vs API Gateway
API routing and composition
整合请求
协议转换
- Authentication—Verifying the identity of the client making the request.
- Authorization—Verifying that the client is authorized to perform that particular
operation.
- Rate limiting —Limiting how many requests per second from either a specific cli-
ent and/or from all clients.
- Caching—Cache responses to reduce the number of requests made to the services.
- Metrics collection—Collect metrics on API usage for billing analytics purposes.
- Request logging—Log requests.
## Why use an API Gateway
如何解决面对多种类型的客户端
微服务的情况下避免一个请求依赖多个服务调用 （BFF with GraphQL）
避免客户端依赖微服务接口导致升级会有冲突

统一管理请求，负载均衡
## BFF
BFF是每个客户端服务独立一套API Gateway，BFF之间互相隔离
BFF服务通常会有客户端团队来维护
## Benefits and drawbacks of an API gateway
### Benefits
   - 统一封装业务接口，客户端侧无须分散请求。
   - 由于是请求的入口，所以可支持功能扩展（如协议转换，限流等）
### Drawbacks
   - 多了一层API Gateway需要保证其可用性足够高，避免服务不可用情况
   - 从开发者角度API发生变化时，需要多更新API Gateway服务
# How 

需求点: 获取歌单详情 （包含创作者信息，歌曲信息，歌单信息）

## How API Gateway Design
Performance and scalability
    Nonblocking I/O
## Cases In Work
### xxx Company which I was stay before
公司内部提供API Gateway平台主要是提供负载均衡和协议转换的功能。业务方通过Thrift IDL文件来创建API Gateway服务。从服务端团队和客户端团队角度分别来说一下使用方式，客户端团队可以自己维护一个BFF服务来做接口的聚合并且暴露给客户端服务使用，通常是使用NodeJS+Thrift协议来实现；服务端团队使用方式则是需要可以直接在业务层实现接口聚合通过暴露接口API Gateway来暴露接口。接口的更新基本需要保证兼容旧版本。
### Netflix

# Reference
- Microservices Patterns With examples in Java - Chapter 8 External API patterns.
- https://en.wikipedia.org/wiki/API [1]

- https://wikitech.wikimedia.org/wiki/API_Gateway

- https://netflixtechblog.com/zuul-2-the-netflix-journey-to-asynchronous-non-blocking-systems-45947377fb5c