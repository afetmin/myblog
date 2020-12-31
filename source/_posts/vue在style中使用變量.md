---
title: vue在style中使用變量
date: 2020-12-31 11:12:08
tags: 进阶
categories: vue
---

## 在style中使用js变量
```html
<template>
  <div class="box" :style="styleVar">
  </div>
</template>
<script>
export default {
  props: {
    height: {
      type: Number,
      default: 54,
    },
  },
  computed: {
    styleVar() {
      return {
        '--box-height': this.height + 'px'
      }
    }
  },
}
</script>
<style scoped>
.box {
  height: var(--box-height);
}
</style>
```
将变量在行内注入，在style中使用var获取js变量。