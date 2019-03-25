---
layout: post
header-img: "img/post-bg-dreamer.jpg"
header-mask: 0.4
author: 'Shiyanping'
title: JavaScript 内功心法——词法作用域及动态作用域
subtitle: 前端必备内功之词法作用域
catalog: true
tags:
  - JavaScript
  - JavaScript 内功心法
---

JavaScript 采用词法作用域，词法作用域也可以理解成静态作用域。与静态作用域相对应的还有动态作用域。

- 静态作用域：函数的作用域在函数定义的时候就已经决定了。
- 动态作用域：函数的作用域只有在使用的时候才决定。

## 1. 静态作用域

举个例子：

```js
var name = 'Tom';

function foo() {
  console.log(name);
}

function bar() {
  var name = 'Jerry';
  foo();
}

bar(); // Tom
```

会输出 Tom，因为静态作用域中，函数的作用域在函数定义的时候就已经决定了，当我们在 foo 中输出 name 时，因为函数 foo 中没有，会逐层往上找，找到了 foo 上一层作用域——全局作用域，全局作用域中 name 是 Tom，所以不管函数在哪执行，输出 name 时都会输出 Tom。

## 2. 动态作用域

当我们使用一种动态作用域的语言时，打印的结果就会不一样了。比如 bash。

自己创建一个 scope.bash，将下面代码 copy 进去，然后在命令行中执行 `bash scope.bash`。

```bash
value=1
function foo () {
    echo $value;
}
function bar () {
    local value=2;
    foo;
}
bar
```

我们得到的结果就是 2 了。因为动态作用域中函数的作用域会在执行的时候才确定。

## 3. 深入理解静态作用域

```js
var scope = 'global scope';
function checkscope() {
  var scope = 'local scope';
  function f() {
    return scope;
  }
  return f();
}
checkscope();
```

```js
var scope = 'global scope';
function checkscope() {
  var scope = 'local scope';
  function f() {
    return scope;
  }
  return f;
}
checkscope()();
```

这两段代码的执行结果都是 local scope，因为 js 属于静态作用域，上面我们也说了，静态作用域中函数作用域是在定义的时候决定的，不管函数何时何地执行，绑定的作用域依然有效。

两个例子中，我们定义函数 f 都是在 checkscope 内部，访问的变量 scope，都是局部变量 scope。

> 上面的例子主要出自[冴羽的博客](https://github.com/mqyqingfeng/Blog/issues/3)，自己写文章主要是为了加深记忆，并且后续复习的时候方便。
