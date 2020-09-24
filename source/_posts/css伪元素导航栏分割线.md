---
title: css伪元素导航栏分割线
date: 2020-05-27 19:23:59
tags: css
categories: css
---
### 分割线伪类
```css
/*使用伪元素制作导航列表项分隔线*/
.nav li:after{
  position:absolute;
  content:"|";
  right:0;
  top:0;
  color:#fff;
  text-shadow:0 0 0 rgba(0,0,0,.5);
}
/*删除最后一项导航分隔线*/
.nav li:last-child::after{
  content:"";
 }
 ```