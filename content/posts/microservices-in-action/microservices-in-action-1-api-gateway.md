---
title: "Microservices in Action 1 - API Gateway"
date: 2023-07-02T20:54:35+08:00
author: "cboy"
tags: ["Microservice", "Programming"]
draft: false
---
# Introduction
分析一下微服务架构下常用的API Gateway，在单体服务是会质疑为什么需要在服务前再套一层网关服务，但是在微服务架构下API Gateway的需求就会出现，现在就来分析和讨论一下它。
# What & Why
在单体服务的情况下，用户请求会直接通过负载均衡网关打到对应的单体业务服务器上，因为单体业务服务器上会包含几乎全部的业务代码，所以涉及到多模块调用时，只需要本地调用方法即可。而在微服务架构下，原本的单体服务会根据一定的规则（例如业务实体）拆分成多个服务，就需要考虑跨服务的多请求问题。举个具体的例子，用户想要请求歌单列表（假设歌单列表请求中需要包含用户信息，歌曲信息，歌单信息）。在单体服务情况下，用户客户端进行一次请求，服务器直接通过本地方法调用并做聚合返回给用户即可。在微服务架构下（假设用户服务，歌曲服务和歌单服务是拆分开的），可能需要分别向用户服务，歌曲服务和歌单服务发出三次请求。为了避免客户端请求三次（公网访问延迟导致用户体验下降，服务也更容易达到公网带宽瓶颈），抽象出API Gateway层来做请求的统一聚合，API Gateway本身就是一层服务，用户客户端只需要直接通过一次请求API Gateway服务获取歌单列表。（补充说明：API Gateway本身也是需要请求三次来进行聚合并返回给客户端的，但是API Gateway的请求走的是Local Area Network(LAN)，所以理论上内网带宽和延迟上都远优于公网环境）
## API Gateway功能
### 必要的功能
- 聚合请求暴露接口供用户客户端调用 
### 其它可选的功能
- 协议转换
- 边缘服务 edge function
  - Authentication—Verifying the identity of the client making the request.
  - Authorization—Verifying that the client is authorized to perform that particular
  operation.
  - Rate limiting —Limiting how many requests per second from either a specific cli-
  ent and/or from all clients.
  - Caching—Cache responses to reduce the number of requests made to the services.
  - Metrics collection—Collect metrics on API usage for billing analytics purposes.
  - Request logging—Log requests.
## BFF
BFF的场景会出现在当客户端包含多种类型时，例如客户端包含移动端，Web网页端，第三方接口调用方。此时对于不同端可以考虑使用不同的API Gateway，而每端都采用独立的API Gateway可以称服务于各端的API Gateway为BFF(Backend for frontend)。
BFF之间互相隔离，BFF服务通常会由各自的客户端团队来维护。
## Benefits and drawbacks of an API Gateway
### Benefits
   - 统一封装业务接口，客户端侧无须分散请求。
   - 由于是请求的入口，所以可支持功能扩展（如协议转换，限流等）
### Drawbacks
   - 多了一层API Gateway需要保证其 可用性足够高，避免服务不可用情况
   - 从开发者角度API发生变化时，需要多更新API Gateway服务
# How 
## 需求
- 获取歌单详情（包含创作者信息，歌曲信息，歌单信息）
- 用户鉴权
- 接口限流
## 实现
### 接口
#### API Gateway
- 获取歌单详情
#### 用户服务
- 获取创作者
#### 歌曲服务
- 获取歌曲
#### 歌单服务
- 获取歌单
### 数据表
使用MySQL作为数据库，数据表如下:
```sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE user (
    `id` bigint unsigned NOT NULL AUTO_INCREMENT COMMENT '用户id',
    `name` varchar(128) NOT NULL COMMENT '用户名',
    `create_time` datetime NOT NULL COMMENT '创建时间',
    `update_time` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

DROP TABLE IF EXISTS `song`;
CREATE TABLE song (
    `id` bigint unsigned NOT NULL AUTO_INCREMENT COMMENT '歌曲id',
    `user_id` bigint unsigned NOT NULL COMMENT '创作者id',
    `song_name` varchar(128) NOT NULL COMMENT '歌名',
    `cover` varchar(512) NOT NULL DEFAULT '' COMMENT '歌曲封面',
    `duration` varchar(32) NOT NULL COMMENT '歌曲时长',
    `download_link` varchar(512) NOT NULL COMMENT '歌曲下载链接',
    `create_time` datetime NOT NULL COMMENT '创建时间',
    `update_time` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

DROP TABLE IF EXISTS `playlist`;
CREATE TABLE playlist (
    `id` bigint unsigned NOT NULL AUTO_INCREMENT COMMENT '歌单id',
    `name` varchar(128) NOT NULL COMMENT '歌单名称',
    `song_id_list` JSON NOT NULL DEFAULT '[]' COMMENT '歌曲id列表',
    `create_time` datetime NOT NULL COMMENT '创建时间',
    `update_time` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
### 实现细节
- 需要考虑到调用失败时如何处理的问题？
- 需要考虑把edge functions哪些功能实现在API Gateway上？

## 工作中使用过的API Gateway
### xxx Company
公司内部提供API Gateway平台主要是提供请求路由和协议转换的功能。业务方通过Thrift IDL文件来创建API Gateway服务。从服务端团队和客户端团队角度分别来说一下使用方式，客户端团队可以自己维护一个BFF服务来做接口的聚合并且暴露给客户端服务使用，通常是使用NodeJS+Thrift协议来实现；服务端团队使用方式则是需要可以直接在业务层实现接口聚合通过暴露接口API Gateway来暴露接口。接口的更新基本需要保证兼容旧版本。
## Open Source Projects
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