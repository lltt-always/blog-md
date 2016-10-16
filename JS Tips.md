---
title: JS Tips
tags: Javascript
---

Javascript技巧小贴士

<!--more-->

1. document.getElementsByName()获得的是数组  
   document.getElementById()获得的是一个元素

2. string.replace(/&/gi, "&amp;") //g表示全局替换；i表示忽略大小写

3. 一个自动调用的方法 toString
在执行一些特殊方法的时候，如 alert, console, innerHTML,脚本解析器会自动检查alert对象的toString方法并调用，因此可以重写toSting方法得到你期望的输出内容与格式。

4. window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行。 
$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。   

# rest（...运算符） 参数与 arguments

- arguments的缺点
  
  - 变量名硬编码使得内层函数想要访问外层函数的arguments需要重新声明一个变量，取得外层函数的arguments值
  - arguments只是一个类数组对象，不能调用数组对象的方法，如.map()、.forEach()等，需要借助Array.prototype.map.call方法来使用。
  - 当使用Array.prototype.push方法时，只能一个数据一个数据的push，不能整个数组push到目标数组

- rest的优点

  - 改善所有arguments的缺点
  - 方便快捷的构造数组、析构数组，将数组元素用于函数参数

注意：rest参数需要是参数列表中的最后一个参数

# 查看浏览器内核

navigator.userAgent

