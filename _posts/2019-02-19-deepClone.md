---
layout: post
header-img: 'img/post-bg-dreamer.jpg'
header-mask: 0.4
author: 'Shiyanping'
title: 浅拷贝和深拷贝
subtitle: 深入理解对象的拷贝方法
tags:
  - JavaScript
---

对象在引用的时候引用的是对象的地址，所以导致如果修改其中一个对象，就会对其他引用这个地址的对象进行修改，这种结果并不是我们想要的，这个时候我们就要用到深拷贝和浅拷贝去解决这个问题了。

```js
var a = {
  name: 'test'
};
var b = a;
a.name = 'test1';
console.log(b); // {name: 'test1'}
```

## 浅拷贝

- Object.assign

Object.assign 只会拷贝所有的属性值到新的对象中，如果属性值是对象的话，拷贝的是地址，所以并不是深拷贝。

```js
var a = {
  name: 'test'
};
var b = Object.assign({}, a);
a.name = 'test1';
console.log(b); // {name: 'test'}
```

- ...扩展运算符

我们可以通过扩展运算符实现浅拷贝。

```js
var a = {
  name: 'test'
};
var b = { ...a };
a.name = 'test1';
console.log(b); // {name: 'test'}
```

## 深拷贝

最简单的深拷贝可以通过 `JSON.parse(JSON.stringify(obj))` 实现。

```js
let a = {
  age: 1,
  jobs: {
    first: 'FE'
  }
};
let b = JSON.parse(JSON.stringify(a));
a.jobs.first = 'native';
console.log(b.jobs.first); // FE
```

但是如果对象中包含 undefined，Symbol，序列化函数，循环引用对象这几种情况的话，这个方法就不起作用了。

```js
let a = {
  age: undefined,
  sex: Symbol('male'),
  jobs: function() {},
  name: 'yck'
};
let b = JSON.parse(JSON.stringify(a));
console.log(b); // {name: "yck"}
```

深拷贝我们还可以使用 MessageChannel，这个方法可以处理 undefined 和循环引用对象。

```js
function structuralClone(obj) {
  return new Promise(resolve => {
    const { port1, port2 } = new MessageChannel();
    port2.onmessage = ev => resolve(ev.data);
    port1.postMessage(obj);
  });
}

var obj = {
  a: 1,
  b: {
    c: 2
  }
};

obj.b.d = obj.b;

// 注意该方法是异步的
// 可以处理 undefined 和循环引用对象
const test = async () => {
  const clone = await structuralClone(obj);
  console.log(clone);
};
test();
```

最简单的我们还可以使用 [lodash 的深拷贝方法](https://lodash.com/docs/4.17.11#cloneDeep)。

如果想自己实现一个深拷贝，其实需要考虑多个边界情况，比较复杂，参考网上的内容，实现一个简易版的深拷贝。

```js
function deepClone(obj) {
  function isObject(o) {
    return (typeof o === 'object' || typeof o === 'function') && o !== null;
  }

  if (!isObject(obj)) {
    throw new Error('非对象');
  }

  let isArray = Array.isArray(obj);
  let newObj = isArray ? [...obj] : { ...obj };
  Reflect.ownKeys(newObj).forEach(key => {
    newObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key];
  });

  return newObj;
}

let obj = {
  a: [1, 2, 3],
  b: {
    c: 2,
    d: 3
  }
};
let newObj = deepClone(obj);
newObj.b.c = 1;
console.log(obj.b.c); // 2
```
