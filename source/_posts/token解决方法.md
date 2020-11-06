---
title: hexo部署时出现excepted token解决方法
date: 2020-11-06 13:40:42
tags: hexo
categories: 错误
---

## hexo部署时出现json解析错误
部署时出现 excepted token x in position，很奇怪，找到源代码有一句```json.parse```,打印了一下发现是某个json对象传入了json.parse。但是这个json对象还必须要有。最后想到了skip_render，发现已经配置了，但是还是渲染的了那些文件。
```
skip_render:
  - "somedir/*"
```
最后去github hexo的 issue 里找到了一个这个：
```
  - "somedir/**/*"
```
怀疑是不是需要两个 ** ，尝试了一下，成功了。