---
title: "如何搭建个人博客"
date: 2023-05-29T00:16:23+08:00
author: "cboy"
tags: ["blog", "hugo", "github-pages"]
---

# 搭建
## 梳理需求
- 互联网能访问到的服务器
- Website

## 选型
### 服务器
- (推荐)[Github Pages](https://docs.github.com/en/pages/getting-started-with-github-pages)服务
- 购买云服务器自建
### Static Website Engine
- (推荐)Hugo
[Github Topic: static-site-generator](https://github.com/topics/static-site-generator)
[Hugo documentation] (https://gohugo.io/documentation/)
**Static Site Theme**
[Hugo Themes] (https://themes.gohugo.io/)

## 扩展功能
### Github CICD
配置Trigger（例如：git push）自动集成和部署
[Cboy's Space Github CICD example file] (https://github.com/yuhangbin/yuhangbin.github.io/blob/main/.github/workflows/hugo.yml)
### 评论
**Gisgus**


### 数据统计
**Google Analytics**
(Google Analytics)[https://analytics.google.com/]注册账号并创建数据流
```yml
# 只需要在config中配置
googleAnalytics: 衡量 ID
```

### 自选域名（包括备案）
**步骤如下**
1. 域名购买
2. 域名解析-CNAME配置
3. Github仓库配置Custom domain 
参考: [Configuring a custom domain for your GitHub Pages site] (https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
### Markdown编辑器
**VSCode**
[VSCode Markdown Documentation] (https://code.visualstudio.com/docs/languages/markdown)

# 为什么搭建博客？
## 搭建动机历史
### 2017
2017年找实习时，觉得在简历上带上博客感觉会很有帮助，还能体现自己的学习历程。由于是第一次搭建加上自己对相关的背景知识匮乏，
东看看西看看，最后选择了Hexo + Github Pages（那时的我还不懂Github有什么用直到我发现可以当博客服务）
### 2021
2021年工作有一段时间了，自己想写一些东西，想把学习轨迹和经历想要记录下来。当时并没有把博客搭建起来，而是把想写的内容记录在Notion中。
### 2023
2023年平时把内容记录到Notion中但是没有人一起交流，在互联网上给自己的内容搭个可以交流的平台。
虽然有各种知识分享的平台可以直接使用，但是还是想搭建一个自己能高度自定义的平台来使用。
