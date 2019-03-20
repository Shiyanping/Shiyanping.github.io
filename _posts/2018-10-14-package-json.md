---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: package.json 使用指南
subtitle: 让你更了解 package.json
catalog: true
date: 2018-10-14
tags:
  - Node
---

## 1. 概述

一般的 node 项目，或者依据 node 搭建的项目，项目的根目录下都会有一个`package.json`文件，`package.json`中定义了项目所需要的各种模块和项目的配置信息。

`package.json`可以手动创建，也可以使用`npm init`去自动生成。

一个完整的`package.json`一般都包含以下内容

```json
{
  "name": "hello world",
  "version": "1.0.0",
  "author": "小石",
  "description": "一个node程序",
  "keywords": ["node.js", "js"],
  "scripts": {
    "start": "gulp",
    "build:js": "gulp uglify-js"
  },
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.1.2",
    "@babel/preset-env": "^7.1.0"
  },
  "dependencies": {
    "browser-sync": "^2.23.6"
  }
}
```

其中`name`，`version`（遵循大版本，次要版本，小版本），`description`，`author`，`keywords`，都属于`package.json`的一些基本信息，主要的功能还是依赖`scripts`，`dependencies`，`devDependencies`。

## 2. scripts

`scripts`主要指定了一些脚本命令的缩写，比如上面的例子中，使用`npm start`就可以运行`gulp`的脚本命令；使用`npm run build:js`就可以运行`gulp uglify-js`的脚本命令。

有些命令缩写需要写`run`修饰符，有些不需要，主要还是看命令的缩写是不是`npm`的关键字。可以在小黑窗（命令行工具）中输入`npm`查看具体`npm`的关键字。

**如：**

![](http://cdn.jinyueyue.cn/15469149827642.jpg)

蓝框框起来的命令，在使用的时候都不需要写`run`，直接输入`npm XX`即可运行指定的命令。

在使用`scripts`指定脚本缩写的时候还有一些小技巧。

### 2.1 scripts 命令的生命周期

在编写运行脚本命令缩写时，有生命周期的说话，在某个脚本缩写之前使用`pre`，当运行指定的脚本命令之前，会先将对应添加了 pre 的命令进行运行。

**如：**

```json
"scripts": {
    "pree2e": "karma start",
    "e2e": "node ./e2e/*.spec.js"
},
```

给`e2e`命令之前添加了一条`pree2e`的命令，在执行`npm run e2e`的时候，就会先执行`pree2e`对应的命令，然后执行 "e2e"对应的命令。

小黑窗中执行的命令就如下图所示：

![](http://cdn.jinyueyue.cn/15469156775730.jpg)

### 2.2 && 和 & 的使用

在编写`scripts`命令时，如果多条命令想写到一个缩写中，可以使用`&&`和`&`符号。

使用`&&`连接的命令属于串行执行，先执行`&&`之前的命令，再执行`&&`之后的。

使用`&`连接的命令属于并行执行，互不干扰，命令同时进行。

**如：**

```json
"scripts": {
    "e2e": "karma start && node ./e2e/*.spec.js",
    "unit": "karma start & node ./e2e/*.spec.js"
},
```

### 2.3 多条命令一起运行

很大的项目中有的时候会有很多条`scripts`命令的缩写，有的时候需要将多条命令同时执行，可以使用上面说的`&&`和`&`符号连接，但是当很多条命令一起执行时，使用`&&`和`&`连接，看起来就会很冗余。

这个时候就要用到`npm-run-all`命令了。

**如：**

```json
"scripts": {
    "test": "npm-run-all unit  e2e  ui  service",
    "unit": "karma start",
    "pree2e": "karma start",
    "e2e": "node ./e2e/*.spec.js",
    "ui": "backstop test",
    "service": "node ./mochaRunner"
}
```

`npm-run-all`命令可以规定有些命令可以一起执行，但是这个是串行的执行，有一个命令挂了就会导致命令停止。

这个时候还可以添加一个关键字`--parallel`，表示后面的命令同时执行。

**如：**

```json
 "test": "npm-run-all --parallel unit  e2e  ui  service"
```

### 2.4 命令的退出

在写脚本命令的时候可以使用`exit()`退出，`exit(0)`表示正常退出，`exit(1)`表示非正常退出，其实这些都是 node 中的命令，在 node 中使用更多一些。

```json
 "test": "npm-run-all --parallel unit  e2e  ui  service exit()"
```

> 在 node 中`process.exit(0)`表示正常退出，`process.exit(1)`表示非正常退出，非正常退出可以再处理异常的时候使用。

## 3. dependencies 和 devDependencies

### 3.1 简介

`dependencies`和`devDependencies`中其实都是添加一些项目中需要引用的依赖包。但是两者是有区别的，`dependencies`中依赖包主要是项目运行时所需要的，在打包的时候会将依赖包打到项目中。`devDependencies`主要是项目开发时所需要的依赖模块，这个主要是开发的时候用，打包的时候不会打到项目中。

`dependencies`和`devDependencies`下的依赖都有版本号。在对应的版本号上可以添加各种限定，主要有一下几种：

- 指定版本：比如 1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。
- 波浪号（tilde）+指定版本：比如~1.2.2，表示安装 1.2.x 的最新版本（不低于 1.2.2），但是不安装 1.3.x，也就是说安装时不改变大版本号和次要版本号。
- 插入号（caret）+指定版本：比如 ˆ1.2.2，表示安装 1.x.x 的最新版本（不低于 1.2.2），但是不安装 2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为 0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。
- latest：安装最新版本。

### 3.2 安装

在安装某一个模块时，会根据使用的命令不同，指定依赖包运行的环境。

**如：**

```shell
npm install express --save
npm install express --save-dev
```

使用`--save`会默认将依赖包安装到`dependencies`下，使用`--save-dev`会将依赖包安装到`devDependencies`下。

## 4. bin

bin 项用来指定各个内部命令对应的可执行文件的位置。

**如：**

```json
"bin": {
  "someTool": "./bin/someTool.js"
}
```

有了上面的 bin 之后，我们写`scripts`命令时，就可以进行缩写了。

**如：**

```js
scripts: {
  start: './node_modules/someTool/someTool.js build';
}

// 简写为
scripts: {
  start: 'someTool build';
}
```

## 5. 其他

其他的命令还有很多，上面的主要是常用的，还想详细了解的，可以参考[JavaScript 标准参考教程](http://javascript.ruanyifeng.com/nodejs/packagejson.html)。

其实`package.json`只是一个配置的 json 文件，懂一些基础的就可以了，能使用最重要，开发中最重要的还是项目主体的开发，好多脚手架其实已经帮我们把这些都弄好了，我们要能看懂。
