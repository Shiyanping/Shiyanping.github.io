---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: JavaScript 内功心法——原型与原型链
subtitle: 前端必备内功之原型链
catalog: true
date: 2018-11-01
tags:
  - JavaScript
  - JavaScript 内功心法
---

我们主要从三个方面说原型和原型链，主要是 prototype，`__proto__`，constructor。

受限我们创建一个构造函数，用于后续生成实例。

```js
function Person() {}
Person.prototype.name = 'Tom';
var person1 = new Person();
console.log(person1.name); // Tom
```

## prototype

**每个函数都有一个 prototype 属性，可以在 prototype 上挂载属性和方法**，那么一个函数的 prototype 具体指向哪里呢？

其实，函数的 prototype 具体指向一个对象，这个对象就是后续引用该构造函数创建的实例的原型。

> 原型：每个对象（null 除外）在创建时都会关联另外一个对象，这个对象就是我们所说的原型，每个对象都会从原型上继承属性和方法。

![](http://cdn.jinyueyue.cn/15529852589261.jpg)

用一张图表示构造函数和实例原型就是如图所示。

prototype 引出了构造函数和实例原型的关系，我们如果想看实例和原型关系，这个时候就要引出 `__proto__` 属性了。

## `__proto__`

每个函数有自己的 prototype，每个对象（null 除外）也有个隐式的属性 `__proto__`，这个属性会指向该对象的原型。

```js
function Person() {}
var person1 = new Person();
console.log(person1.__proto__ === Person.prototype); // true
```

person1 这个对象，是我们通过构造函数 Person 实例化出来的对象，上面说 Person.prototype 是一个对象，所有通过 Person 实例化出来的对象，原型都指向 Person.prototype。所以我们就得到了上面的结果。

这样，我们就能将上面的原型图进行进一步更新：

![](http://cdn.jinyueyue.cn/15529860591886.jpg)

其实 `__proto__` 属性并不是所有的浏览器都支持，我们在使用 person1.**proto** 的时候，可以理解成使用了 Object.getPrototypeOf(person1)，获取对象的原型。

## constructor

构造函数通过的 new 方法创建实例，通过 prototype 指向原型。

实例可以通过 `__proto__` 指向原型，其实原型也有属性可以指向构造函数，这个时候就要用到 constructor 属性了，每个原型都有一个 constructor 属性，指向关联的构造函数。

```js
function Person() {}
var person1 = new Person();
console.log(Person === Person.prototype.constructor); // true
```

我们把原型图进一步进行更新：

![](http://cdn.jinyueyue.cn/15529863103184.jpg)

一个简单的原型链是这样，但是，原型是一个对象，我们之前说每个对象都有自己的 `__proto__` 属性，所以当我们使用原型的 `**proto** 属性时，还会继续找原型对应的原型。

## 原型链

```js
function Person() {}
Person.prototype.name = 'Tom';
var person1 = new Person();
person1.name = 'Jerry';
console.log(person1.name); // Jerry

delete person1.name;
console.log(person1.name); // Tom
```

当我们给原型添加一个 name 值，并且给实例 person1 添加一个 name 值之后，如果输出 person1.name 会直接输出 person1 的 name。

当我们把 person1 的 name 属性删除之后，输出 person1.name 并不会报错，反而会输出原型上的 name 值。

这就是因为如果对象上没有某个属性或者方法时，会从对象的原型上去找，如果该对象的原型上没有的话，会逐层接着往上找，直到找到顶层位置。

这个时候原型图就更加丰富了：

![](http://cdn.jinyueyue.cn/15529874922858.jpg)

当我们找到 Object.prototype 上之后就不会往后找了，打印一下 Object.prototype.**proto**，就知道为什么了。

```js
console.log(Object.prototype.__proto__ === null);
```

因为 Object.prototype 的原型是 null，null 就表示“没有对象”，没有值，所以 Object.prototype 再往上就没有原型了，也就是说 Object.prototype 就是顶层的原型。

当对象使用一个方法或者属性时，如果当前对象有，那就直接使用，如果没有会一直找到顶层原型上（Object.prototype）。

将原型图再完整一下，也是我们最后的最终版：

![](http://cdn.jinyueyue.cn/15529878701059.jpg)

我们都是使用 `__proto__` 去查找，通过 `__proto__` 将原型对象关联起来，这就是原型链。

**总结一下原型链的四条规则：**

1. 函数都要自己的 prototype 属性，指向该构造函数创建的实例的原型。
2. 对象都有 `__proto__` 属性，指向原型，也就是指向其构造函数的 prototype。
3. 原型的 constructor 属性，都指向关联的构造函数。
4. 引用类型上如果没有某个属性或者方法时，会通过 `__proto__` 逐层向上去找，知道顶层原型（Object.prototype）。

**参考：**

[冴羽-JavaScript 深入之从原型到原型链](https://github.com/mqyqingfeng/Blog/issues/2)
