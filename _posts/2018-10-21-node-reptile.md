---
layout: post
header-img: "img/post-bg-dreamer.jpg"
header-mask: 0.4
author: 'Shiyanping'
title: Node 爬虫
subtitle: 简单了解 Node 爬虫
catalog: true
tags:
  - Node
---

爬虫，是一种自动获取网页内容的程序，是搜索引擎重要的组成部分，因此搜索引擎优化很大程度上就是针对爬虫而做出的优化。

robots.txt 可以规定爬虫访问的范围，爬虫进来一个网页之后，第一个访问的文件就是 robots.txt，如果我们爬取别人的网站，访问了 robots.txt 不包含的内容，后续出现什么问题，我们爬取的网站会告我们侵权。

爬虫访问的是源文件，爬到的是一个 html 文件。

## 使用 express 爬取网站

在对一个网站进行爬取之前，可以先访问 XX/robots.txt，可以看到这个网站具体不让访问什么内容，当我们访问不让访问的内容时，可能看到的就是 403 报错了。

### 依赖的模块

cheerio
request
express

### 相关代码

```js
const express = require('express');
const app = express();
const request = require('request');
const cheerio = require('cheerio');

app.get('/', (req, res) => {
  request('http://www.baidu.com', function(error, response, body) {
    console.log('error:', error); // Print the error if one occurred
    console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
    console.log('body:', body); // Print the HTML for the Google homepage.
    $ = cheerio.load(body); // 当前的$相当于拿到当前页面整个body的选择器
    res.json({
      input: $('#bri').text()
    });
  });
});

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```
