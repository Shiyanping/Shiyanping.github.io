---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: 大前端必会技能——Node
subtitle: Node 简单概述及思想
catalog: true
date: 2018-10-10
tags:
  - Node
---

在使用 Node 做中间层时，一定要注意如何注册中间件，错误处理，配置，日志。

controller 控制操作和路由，model 主要用作访问真正的后端接口，如果使用 Node 做后端，model 控制和数据库交互。

所有协议 Unix Domain Socket 走 IPC 同时搭载 L5 动态分配集群 IP。

1. 中间件

一个路由下可能会有多个中间件去配合操作完成，中间件中主要包含 request，response，next，主要通过 request 和 response 通信，通过 next 实现，对下一个中间件的过渡。

## 1. 概述

[Node.js](https://nodejs.org/en/) 本身使用的就是 V8 引擎，Chrome 中运行的 JavaScript 也是 V8 引擎。Node.js 主要是 JavaScript 的运行环境。在开发 Node.js 中我们主要使用的 ES6 语法，因为 Node.js 天然支持 ES6。

**Node.js 主要功能：**

- 提供高性能的 Web 服务
- IO 性能强大，所以 Node 适用于 IO 密集型的环境
- 事件处理机制完善
- 天然处理 DOM，使用 Node 做网页爬虫，服务器端渲染比较方便
- 开发的生态圈日趋完善，社区非常活跃

**Node.js 主要优势：**

- 可以处理大流量的数据，IO 密集型
- 很适合实时交互，在线聊天
- 完美的支持对象型的数据库，例：MongoDB
- 异步处理大量的并发连接

## 2. 事件驱动机制

Node 中如果想要操作事件，那我们就要使用事件驱动机制了，在 Node 中，如果想使用事件驱动，我们需要 EventEmitters 这个 api。

下图主要是一个事件驱动的流程。

![](http://cdn.jinyueyue.cn/15486461962997.jpg)

首先维护一个事件队列 Events，事件队列将事件逐步加到事件轮询 Event Loop 中。

如果事件队列中没有事件需要操作，那 event loop 会进行休眠，会定时对事件队列进行检查，如果有那就运行，被称为事件驱动模型。

例：

```js
// 引用 EventEmitter
const events = require('events');
const eventEmitter = new events.EventEmitter();

// 绑定事件
eventEmitter.on('printTips', () => {
  console.log('事件被执行');
});

// 触发事件
eventEmitter.emit('printTips');

console.log('程序执行完毕');
```

在 Node 中依靠的设计模式主要是观察者模式。

## 3. 模块化

大的项目，我们一般都会使用模块化，易于项目管理，在 Node 中也是这样的，开发 Node 项目时，我们会有不同的功能，不同的方法，所以也会使用到模块化。

为了让 Node 中的文件可以项目调用，Node 中自身带了模块化的一个系统。使用`require`进行模块的引用，使用`module.export`将模块导出。

在 Node 中一个文件表示一个模块。其中模块可以是 C++扩展，json 文件，我们自己编写的 Node 文件。

**Node 中模块化的加载流程：**

![](http://cdn.jinyueyue.cn/15486476098408.jpg)

Node 中加载模块主要分为从缓存中加载，从文件模块中加载，从原生模块中加载，如果模块区有缓存，那么优先使用缓存区的模块，原生模块区和文件模块区是分开的。如果发现没有缓存的话，会从对应的模块区中去找，找到后进行引用，并且放入缓存中，后续使用缓存中的模块。

设置缓存可以节省内存的消耗，并且能够加速模块引入的速度。

## 4. 常用工具

`util.inherits`：继承
`util.inspect` ：将任意对象转换成字符串
`util.isArray(object)`： 判断是不是数组
`util.isRegExp(object)` ：判断是不是正则
`util.isDate(object)`：判断是否为日期
`util.isError(object)`：判断是不是一个错误对象

## 5. 名词解释：

spa：单页面应用

mpa：多页面应用

SSR：服务端渲染

## 6. 项目中的功能：

整合服务端 api

优化性能

可以让前端同学控制整个项目，实现真正的前后端分离

## 7. 异步 IO 原理

Node 适用于 IO 密集型（输入输出），不适用 cpu 密集型（计算性）

tips：

区分 Libuv 和 V8

process.nextTick 处于异步和同步队列之间

V8 垃圾回收机制是分代的，新生代和老生代

## 8. 内存泄露的原因

1. 无限制增长的数据
2. 无限制设置对象的属性
3. 私有变量和方法均是永驻的内存
4. 大循环，没有 GC 的机会

## 9. 大型网站架构

想要弄好大型网站的架构，首先要弄好文件夹目录，最基础的 MVC，想构造更大的项目，可以了解.net，和 java 的项目架构。
