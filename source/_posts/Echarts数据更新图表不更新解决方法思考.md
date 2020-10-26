---
title: Echarts数据更新图表不更新解决方法思考
date: 2020-10-26 16:48:33
tags: vue
categories: vue
---
## echart数据更新问题
在使用vue-echarts组件时，通过watch监听option变化更新数据，用vue调试工具查看数据更新了，series数组内容改变了。
但是echart的线表有部分数据是上次的，有部分是最新的。百思不得其解。
最后突然想到vue的组件会复用，没有变化的不更新。最后通过强制更新图表插件解决问题。

这里记录一下四种更新数据的方法

### 刷新整个页面（最low的，可以借助route机制）
### 使用v-if标记（比较low的）
### 使用内置的forceUpdate方法（较好的）
组件内置$forceUpdate方法，使用前需要在配置中启用。
```js
import Vue from 'vue'
Vue.forceUpdate()

export default {
  methods: {
    handleUpdateClick() {
      // built-in
      this.$forceUpdate()
    }
  }
}
```
### 使用key-changing优化组件（最好的)
原理很简单，vue使用key标记组件身份，当key改变时就是释放原始组件，重新加载新的组件。
```html
<template>
  <div>
    <span :key="key"></span>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        key: 0
      }
    },
    methods: {
      handleUpdateClick() {
        this.key += 1
      }
    }
  }
</script>
```

