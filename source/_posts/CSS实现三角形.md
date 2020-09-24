---
title: CSS伪类实现三角形
date: 2020-06-02 10:16:56
tags: css
categories: css
index_img: /img/CSS伪类实现三角形/tri3.png
---
## CSS实现三角形

```html
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css" id="colorFlipFlop">
  * {margin:0; padding: 0;}
  .demo {
    width: 100px;
    height: 100px;
    position: relative;
    border: 1px solid #09f;
    margin: 100px;
  }
  .demo:after {
    width: 0px;
    height: 0px;
    border-width: 12px;
    border-style: solid;
    /* 上 右 下 左 */
    border-color: transparent transparent #933 transparent;
    position: absolute;
    content: ' ';
    left: 39px;
    top: -24px;
  }
</style>
</head>
<body>
  <div class="demo"></div>
</body>
</html>
```
### 实心三角形
```html
<!DOCTYPE html>
<html>
   <head>
       <meta charset="UTF-8">
       <title></title>
       <style type="text/css">
           .tri_top, .tri_right, .tri_bottom, .tri_left{
               width: 150px;
               height: 100px;
               background: #CCCCCC;
               border-radius: 8px;
               margin: 50px 50px;
               position: relative;
               float: left;
           }
           .tri_top:before{
               content: "";
               width: 0px;
               height: 0px;
               border-left: 10px solid transparent;
               border-right: 10px solid transparent;
               border-bottom: 10px solid #CCCCCC;
               position: absolute;
               top: -10px;
               left: 65px;
           }
           .tri_right:before{
               content: "";
               width: 0px;
               height: 0px;
               border-top: 10px solid transparent;
               border-bottom: 10px solid transparent;
               border-left: 10px solid #CCCCCC;
               position: absolute;
               top: 40px;
               left: 150px;
           }
           .tri_bottom:before{
               content: "";
               width: 0px;
               height: 0px;
               border-top: 10px solid #CCCCCC;
               border-left: 10px solid transparent;
               border-right: 10px solid transparent;
               position: absolute;
               top: 100px;
               left: 70px;
           }
           .tri_left:before{
               content: "";
               width: 0px;
               height: 0px;
               border-top: 10px solid transparent;
               border-bottom: 10px solid transparent;
               border-right: 10px solid #CCCCCC;
               position: absolute;
               top: 40px;
               left: -10px;
           }
       </style>
   </head>
   <body>
       <div class="tri_top"></div>         <!--三角形在上边-->
       <div class="tri_right"></div>       <!--三角形在右边-->
       <div class="tri_bottom"></div>      <!--三角形在底边-->
       <div class="tri_left"></div>        <!--三角形在左边-->
   </body>
</html>
```
![实心三角形](/img/CSS伪类实现三角形/tri.png)
### 空心三角形
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            .tri_top, .tri_right, .tri_bottom, .tri_left{
                width: 150px;
                height: 100px;
                border: 1px solid #000000;
                border-radius: 8px;
                margin: 50px 50px;
                position: relative;
                float: left;
            }
            .tri_top:before{
                content: "";
                width: 0px;
                height: 0px;
                border-left: 15px solid transparent;
                border-right: 15px solid transparent;
                border-bottom: 15px solid #000000;
                position: absolute;
                top: -15px;
                left: 65px;
            }
            .tri_top:after{
                content: "";
                width: 0px;
                height: 0px;
                border-left: 14px solid transparent;
                border-right: 14px solid transparent;
                border-bottom: 14px solid #FFFFFF;
                position: absolute;
                top: -14px;
                left: 66px;
            }
            .tri_right:before{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 15px solid transparent;
                border-bottom: 15px solid transparent;
                border-left: 15px solid #000000;
                position: absolute;
                top: 39px;
                left: 150px;
            }
            .tri_right:after{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 14px solid transparent;
                border-bottom: 14px solid transparent;
                border-left: 14px solid #FFFFFF;
                position: absolute;
                top: 40px;
                left: 149px;
            }
            .tri_bottom:before{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 15px solid #000000;
                border-left: 15px solid transparent;
                border-right: 15px solid transparent;
                position: absolute;
                top: 101px;
                left: 69px;
            }
            .tri_bottom:after{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 14px solid #FFFFFF;
                border-left: 14px solid transparent;
                border-right: 14px solid transparent;
                position: absolute;
                top: 100px;
                left: 70px;
            }
            .tri_left:before{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 15px solid transparent;
                border-bottom: 15px solid transparent;
                border-right: 15px solid #000000;
                position: absolute;
                top: 40px;
                left: -15px;
            }
            .tri_left:after{
                content: "";
                width: 0px;
                height: 0px;
                border-top: 14px solid transparent;
                border-bottom: 14px solid transparent;
                border-right: 14px solid #FFFFFF;
                position: absolute;
                top: 41px;
                left: -14px;
            }
        </style>
    </head>
    <body>
        <div class="tri_top"></div>         <!--三角形在上边-->
        <div class="tri_right"></div>       <!--三角形在右边-->
        <div class="tri_bottom"></div>      <!--三角形在底边-->
        <div class="tri_left"></div>        <!--三角形在左边-->
    </body>
</html>
```
![空心三角形](/img/CSS伪类实现三角形/tri2.png)

总结：
 三角形往哪个方向，那个方向无需设置border，而相反方向设置border颜色，相邻两边的border设为透明。这样就可实现各个方向的三角形。实心三角形利用CSS中的伪元素（：before）实现，再利用border的transparent属性即可达到效果。而空心三角形是在空心三角形的基础上再加上伪元素（：after）实现。伪元素（：before）实现的是一个实心的三角形，伪元素（：after）实现的是空心的三角形，进而把实心的三角形覆盖，利用绝对定位的top与left的差值绝对了三角形线的粗细而达到如图的效果。
