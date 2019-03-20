---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: 页面加载、性能优化、XSS攻击
subtitle: 增加前端内功
catalog: true
date: 2018-04-21 21:32:53
tags:
  - 性能优化
---

**介绍一下页面加载，性能优化，及简单的 web 前端攻击的知识，只是简单的介绍昂，不喜勿喷
知识点包括：从输入 url 到渲染的整个过程介绍、页面优化，缓存、页面加载方式等**

<!-- more -->

## 一、知识点

### 加载一个资源的过程（从输入 url 得到 html 的渲染过程）

- 浏览器根据 DNS 服务器得到域名的 IP 地址
- 向 ip 对应的机器发送 http 请求
- 服务器得到请求，处理并返回 http 资源
- 浏览器得到资源后对资源进行处理返回内容

### 浏览器渲染页面的过程

- 根据 HTML 生成 DOM tree
- 根据 CSS 生成 CSSOM
- 根据 DOM 和 CSSOM 生成 RenderTree
- 根据 renderTree 渲染页面和展示
- 遇到 script 会执行并且阻塞渲染

### 页面优化的原则

- 多使用内存，缓存
- 减少网络请求

#### 缓存的分类

- 强缓存

  1. Expires: 绝对时间
  2. Cache-Control: 相对时间 (秒)

  > 如果两个时间都有，则以后者为准

- 协商缓存

  1. Last-Modified (服务器下发)，if-Modified-Since (校验绝对时间)
  2. Etag，if-None-Match

### 页面优化

- 页面资源优化
  - 静态资源压缩合并
  - 静态资源缓存
  - 使用 CDN 让资源加载更快
  - 使用 SSR 后端渲染，数据直接输出到 HTML 中
- 渲染优化
  - css 放前面，JS 放后面
  - 懒加载（图片懒加载，上拉加载更多）
  - 减少 DOM 查询，对 DOM 查询进行缓存
  - 减少 DOM 操作，多个操作尽量合并在一起
  - 事件节流
  - 尽早进行操作（DomContentLoaded）

### 前端攻击类型

- XSS 跨域脚本攻击

  - 插入一段`<script>`
  - 解决办法：前端主动将`<`，`>`进行转义替换，但是会影响性能

- CSRF 跨站请求伪造

  - 攻击原理：
    ![攻击原理](/images/15264816592378.jpg)

  - 防御措施
    - 添加 token 校验
    - referer 验证（判断页面来源）
    - 隐藏令牌

### 算法

- 排序
  - 快排：https://segmentfault.com/a/1190000009426421
  - 选择排序：https://segmentfault.com/a/1190000009366805
  - 希尔排序：https://segmentfault.com/a/1190000009461832
- 堆栈、队列、链表  
  https://juejin.im/entry/58759e79128fe1006b48cdfd
- 递归  
  https://segmentfault.com/a/1190000009857470
- 波兰式和逆波兰式

## 二、问题

### window.onload 和 DomContentLoaded 的区别

- window.onload 是所有元素都加载完，包括图片，视频等大的资源

- DomContentLoad 是 DOM 渲染完之后，不包括图片和视频渲染完
