---
title: px、em、rem与百分比
tags: CSS
---

说说css中的各种尺寸

<!--more-->

# px、em、rem与百分比

  px、em、rem与百分比都是css中的相对长度单位，那么他们之间有什么区别呢？

## px

  像素，即计算机屏幕上的一个点，相对于显示器分辨率而言
  当使用浏览器字体放大功能时无效

## rem  
   当使用 rem 单位，他们转化为像素大小取决于页根元素的字体大小，即 html 元素的字体大小。 根元素字体大小乘以 rem 值。

## em
   当使用em单位时，像素值将是em值乘以使用em单位的元素的字体大小。

## rem 与 em
   rem适用于整个页面布局，这样当调整浏览器字体大小时，整个页面有更好的适应性，特别适用于：字体设置、媒体查询设置
   em取决于font-size，而非html，因此更适用于局部布局，使用某个元素的字体大小做缩放。

 - 注：多列布局的列宽不要使用rem与em，而尽可能的使用%；单一列可以使用rem；当缩放会不可避免的导致打破布局元素时，不要使用rem或em

   em的继承
   使用em的过程中存在继承的问题，如下例所示

   ```
   html font-size: 16px
       div padding: 1.5em //1.5*16=24px
   
   html font-size: 16px
       div font-size: 1.25em //1.25*16=20px
           div padding: 1.5em //1.5*20=30px

   html font-size: 16px
       div font-size: 1.25em //1.25*16=20px
           div font-size: 1.2em //1.2*20=24px
               padding: 1.5em //1.5*24=36px

   html font-size: 16px
       div font-size: 1.25em //1.25*16=20px
           div font-size: 14px
               padding: 1.5em //1.5*14=21px
   ```

   根字体大小与浏览器默认字体
   （1）未显式设置根字体大小
        html font-size = 浏览器设置
   （2）用em单位显式设置根字体大小为x em
        html font-size = x * 浏览器设置
   （3）用px单位显式设置根字体大小
        不推荐！！！

  
  