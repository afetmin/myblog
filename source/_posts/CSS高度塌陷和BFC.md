---
title: CSS高度塌陷和BFC
date: 2020-06-15 11:43:11
tags: CSS
categories: CSS
index_img: /img/CSS高度塌陷和BFC/高度塌陷.png
---
## 高度塌陷
当子元素浮动时，脱离了文档流，导致父元素高度撑不起来。
这时可以通过在在子元素后面添加内容，并且清楚它的浮动来使其跟在浮动元素的后面，这样就可以撑起父元素的高度了。
`子元素`加上下面这个 `clearfix` 类就可以了。
```css
clearfix::after {
  content: '';
  display: block;
  clear: both;
}
```
## 相邻元素的外边距重叠
子元素与父元素，当给子元素设置   `margin-top` 时，子元素会与父元素一起向下移动，解决方法时使两个元素外边距不重叠。这时可以给`父元素`加上下面的 `clearfix` 类来解决。
```css
clearfix::before {
  content: '';
  display: table;
}
```
上面两个可以合成一个：
```css
clearfix::before,
clearfix::after {
  content: '';
  display: table;
  clear: both;
}
```
## BFC
BFC`(Block Formatting Context)`块级格式化上下文。具体解释见 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
这是解决高度塌陷的背后原理，是CSS里面隐藏的属性。
高度塌陷还可以通过使父元素生成BFC来解决，通过给父元素添加
```css
{
  overflow: hidden;
}
```

## 图片自适应问题
通过这样写css，可以直接获取图片高度，而不用等到图片加载出再获得。
```css
position: relative;
width: 100%;
height: 0;
padding-top: 70%;
transfrom-origin: top;
background-size: cover;
```
通过 width:100%和 padding-top:70%控制图片的宽高比为10:7，当页面加载时，宽度确定了，高度也就确定了。

图片宽度设置100%，页面加载时会存在高度塌陷的问题，可以使用padding－top来设置百分比值来实现自适应 padding-top = (图片的高度/图片的宽度)*100。

