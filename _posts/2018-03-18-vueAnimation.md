---
layout: post
header-img: 'img/bg/hello_world.jpg'
author: 'Shiyanping'
title: Vue 中添加动画
subtitle: 深入浅出 Vue 系列
catalog: true
date: 2018-03-18 15:10:27
tags: Vue
---

vue 中的动画主要依靠`transition`这个控件，关于 transition 这个 api 可以上官网查看[vue 中的 transition ](https://cn.vuejs.org/v2/api/#transition)，其中还有用到 animate.css 和 Velocity.js

<!-- more -->

## 一、vue 中使用 css 动画

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Vue 中 CSS 动画原理</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <style>
        .fade-enter,
        .fade-leave-to {
            opacity: 0;
        }

        .fade-enter-active,
        .fade-leave-active {
            transition: opacity 1s;
        }
    </style>
</head>

<body>
    <div id="container">
        <transition name="fade">
            <component :is="tabName"></component>
        </transition>

        <button @click="switchNav">switch</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script>
    Vue.component('child-one', {
        template: `<div v-once>child-one</div>`
    });

    Vue.component('child-two', {
        template: `<div v-once>child-two</div>`
    });

    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                tabName: 'child-one'
            }
        },
        methods: {
            switchNav: function () {
                this.tabName = this.tabName == 'child-one' ? 'child-two' : 'child-one'
            }
        }
    });
</script>

</html>
```

## 二、vue 中 animate.css 动画

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>在Vue中使用 animate.css 库</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <link rel="stylesheet" href="https://cdn.bootcss.com/animate.css/3.5.2/animate.min.css">
</head>

<body>
    <div id="container">
        <transition enter-active-class="animated fadeIn"
                    leave-active-class="animated shake">
            <div v-show="show">animation</div>
        </transition>

        <button @click="switchNav">toggle</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                show: true
            }
        },
        methods: {
            switchNav: function () {
                this.show = !this.show;
            }
        }
    });
</script>

</html>
```

## 三、在 vue 中同时使用过渡和动画

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>在Vue中同时使用过渡和动画</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <link rel="stylesheet" href="https://cdn.bootcss.com/animate.css/3.5.2/animate.min.css">
    <style>
        .fade-enter,
        .fade-leave-to {
            opacity: 0;
        }
        .fade-enter-active,
        .fade-leave-active {
            transition: opacity 2s;
        }
    </style>
</head>

<body>
    <div id="container">
        <transition name="fade"
                    appear
                    :duration="{enter: 1000, leave: 3000}"
                    enter-active-class="animated jello fade-enter-active"
                    leave-active-class="animated tada fade-leave-active"
                    appear-active-class="animated jello fade-enter-active">
            <div v-show="show">animation</div>
        </transition>

        <button @click="switchNav">toggle</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                show: true
            }
        },
        methods: {
            switchNav: function () {
                this.show = !this.show;
            }
        }
    });
</script>

</html>
```

## 四、Vue 中的 Js 动画与 Velocity.js 的结合

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Vue中的 Js 动画与 Velocity.js 的结合</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <link rel="stylesheet" href="https://cdn.bootcss.com/animate.css/3.5.2/animate.min.css">
</head>

<body>
    <div id="container">
        <transition @before-enter="handleBeforeEnter"
                    @enter="handleEnter"
                    @after-enter="handleAfterEnter">
            <div v-show="show">animation</div>
        </transition>

        <button @click="switchNav">toggle</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script src="https://cdn.bootcss.com/velocity/2.0.2/velocity.js"></script>
<script>
    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                show: true
            }
        },
        methods: {
            switchNav: function () {
                this.show = !this.show;
            },
            handleBeforeEnter: function(el) {
                el.style.opacity = 0;
            },
            handleEnter: function(el, done) {
                Velocity(el, {opacity: 1}, {duration: 1000, complete: done});
            },
            handleAfterEnter: function(el) {
                console.log('动画结束');
            }
        }
    });
</script>

</html>
```

## 五、Vue 中多个元组件的过渡

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Vue中多个元组件的过渡</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
    <style>
        .v-enter,
        .v-leave-to {
            opacity: 0;
        }

        .v-enter-active,
        .v-leave-active {
            transition: opacity 1s;
        }
    </style>
</head>

<body>
    <div id="container">
        <transition mode="out-in">
            <component :is="type"></component>
        </transition>

        <button @click="switchNav">toggle</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script>

    Vue.component('child-one', {
        template: '<div>child-one</div>'
    })

    Vue.component('child-two', {
        template: '<div>child-two</div>'
    })

    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                type: 'child-one'
            }
        },
        methods: {
            switchNav: function () {
                this.type = this.type == 'child-one' ? 'child-two' : 'child-one';
            }
        }
    });
</script>

</html>
```

## 六、封装 vue 中的动画组件

```html
<!doctype html>
<html>

<head>
    <meta charset="UTF-8">
    <title>封装vue中的动画组件</title>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0">
</head>

<body>
    <div id="container">
        <fade :show="show">
            <div>hello transition</div>
        </fade>
        <button @click="handleClick">toggle</button>
    </div>
</body>
<script src="https://cdn.bootcss.com/vue/2.5.0/vue.js"></script>
<script>
    Vue.component('fade', {
        props: ['show'],
        template: `
            <transition @before-enter="handleBeforeEnter"
                        @enter="handleEnter">
                <slot v-if="show"></slot>
            </transition>
        `,
        methods: {
            handleBeforeEnter: function(el) {
                el.style.opacity = 0;
            },
            handleEnter: function(el, done) {
                setTimeout(() => {
                    el.style.opacity = 1;
                    done();
                }, 1000);
            }
        }
    })
    var vm = new Vue({
        el: "#container",
        data: function () {
            return {
                show: true
            }
        },
        methods: {
            handleClick: function () {
                this.show = !this.show;
            }
        }
    });
</script>

</html>
```
