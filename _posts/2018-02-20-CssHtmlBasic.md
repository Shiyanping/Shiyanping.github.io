---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: CSS HTML基础
subtitle: 前端基础
catalog: true
date: 2018-02-20 10:20:43
tags:
  - CSS
  - HTML
---

这篇文章主要是说一些前端 css 和 html 的面试题的，有具体想知道 css 和 html 基础的，其实可以从网上百度，一搜一大片，可以去 W3C 看看，讲的还不错

## 一、CSS 盒模型

- 标准模型+IE 模型
  下面用两张图解释标准模型和 IE 模型
  ![](/images/15261811358506.jpg)
  ![](/images/15261811463925.jpg)

<!-- more -->

- 标准模型和 IE 模型的区别

宽度的计算不同，标准盒模型 content 的宽不包括 padding，border，IE 盒模型包括 padding，border

- CSS 如何设置这两种模型

IE 盒模型：box-sizing: border-box
标准盒模型：box-sizing: content-box（默认）

- JS 如何设置获取盒模型的宽和高

  - element.style.width/height：只能取出来内联样式
  - element.currentStyle.width/height：只有 IE 支持
  - window.getComputedStyle(dom).width/height：支持所有浏览器，运行完之后的样式
  - dom.getBoundingClientRect().width/height：包括 left，right，top，bottom，相对于视窗

- BFC（边距重叠解决方案）

  - BFC 的基本概念
    块级格式化上下文

    > IFC(内联格式化上下文)

  - BFC 的原理
    - BFC 元素垂直边距会出现边距重叠
    - BFC 的区域不会与 box 元素重叠
    - BFC 在页面是一个独立的容器，外面的元素不会影响内部的元素
    - 计算 BFC 高度时，浮动元素也会计算
  - BFC 的创建
    - float: 不等于 null
    - display: table-ceil | table
    - position: absoulte | fixed
    - overflow: hidden
  - 使用场景
    - 高度大于兄弟元素，兄弟元素为浮动元素时，高度大的会和 float 元素重叠（设置 BFC 不予 float 重叠）
    - 兄弟元素上下边距取最大值的问题（添加父元素，并设置父元素为 BFC）
    - 子元素为浮动元素，父元素高度不计算的问题（设置父元素为 BFC）

## 问题

### 写一个三栏布局，高度已知，左右各 300px，中间自适应

- float 布局
- table 布局
- position 布局
- flex 布局
- gird 布局(网格布局)

如果是高度自适应的话，可以使用 flex 和 table 布局

#### 扩展

- 三栏布局

  - 左右宽度固定，中间自适应
  - 上下宽度固定，中间自适应

- 两栏布局
  - 左固定，右自适应
  - 右固定，左自适应
  - 上固定，下自适应
  - 下固定，上自适应
