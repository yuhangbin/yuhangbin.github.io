---
title: "Microservices in Action 1 - API Gateway"
date: 2023-07-02T20:54:35+08:00
author: "cboy"
tags: ["Microservice", "Programming"]
draft: true
---
# Introduction
分析一下微服务架构下常用的API Gateway，在单体服务是会质疑为什么需要在服务前再套一层网关服务，但是在微服务架构下API Gateway的需求就会出现，现在就来分析和讨论一下它。
# What & Why
## What is an API Gateway?
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
## Why use an API Gateway?
如何解决面对多种类型的客户端
微服务的情况下避免一个请求依赖多个服务调用 （BFF with GraphQL）
避免客户端依赖微服务接口导致升级会有冲突

统一管理请求，负载均衡
## BFF
BFF是每个客户端服务独立一套API Gateway，BFF之间互相隔离
BFF服务通常会有客户端团队来维护
## Benefits and drawbacks of an API Gateway
### Benefits
   - 统一封装业务接口，客户端侧无须分散请求。
   - 由于是请求的入口，所以可支持功能扩展（如协议转换，限流等）
### Drawbacks
   - 多了一层API Gateway需要保证其可用性足够高，避免服务不可用情况
   - 从开发者角度API发生变化时，需要多更新API Gateway服务
# How 
需求点: 获取歌单详情 （包含创作者信息，歌曲信息，歌单信息）


需要考虑到调用失败时如何处理的问题？
需要考虑把edge functions哪些功能实现在API Gateway上？
    routing, rate limiting and authentication
## How API Gateway Design
Performance and scalability
    Nonblocking I/O
## Cases In Work
### xxx Company which I was stay before
公司内部提供API Gateway平台主要是提供请求路由和协议转换的功能。业务方通过Thrift IDL文件来创建API Gateway服务。从服务端团队和客户端团队角度分别来说一下使用方式，客户端团队可以自己维护一个BFF服务来做接口的聚合并且暴露给客户端服务使用，通常是使用NodeJS+Thrift协议来实现；服务端团队使用方式则是需要可以直接在业务层实现接口聚合通过暴露接口API Gateway来暴露接口。接口的更新基本需要保证兼容旧版本。
## Open Source Projects
### Netflix Zuul
### Spring Cloud Gateway
Kong / Traefik

## API Gateway using GraphQL
使用图数据库优化调用部分

# Reference
- Microservices Patterns With examples in Java - Chapter 8 External API patterns.
- https://wikitech.wikimedia.org/wiki/API_Gateway
- https://netflixtechblog.com/zuul-2-the-netflix-journey-to-asynchronous-non-blocking-systems-45947377fb5c
- https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#glossary
- https://microservices.io/patterns/apigateway.html
- https://netflixtechblog.com/tagged/api-gateway
- https://medium.com/geekculture/load-balancer-vs-reverse-proxy-vs-api-gateway-e9ec5809180c