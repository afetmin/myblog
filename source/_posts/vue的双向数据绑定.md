---
title: vue的双向数据绑定
date: 2020-11-06 13:54:28
tags: vue
categories: 进阶
---

## vue的双向数据绑定
vue数据双向绑定是通过数据劫持结合发布者-订阅者模式的方式来实现的。原理图：
![vue的双向数据绑定](/img/vue数据双向绑定/reactive.jpg)

通过 Object.defineProperty() 方法劫持 对象的 getter、setter方法，在通过发布订阅模式完成 data --> 视图 和 视图 --> data 的双向更新。
具体查看: [vue的双向绑定](https://www.cnblogs.com/canfoo/p/6891868.html)
