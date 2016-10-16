---
title: CSS使div居中
tags: CSS
---

五种使 div 块水平垂直居中的CSS实现方法；文字在div中居中

<!--more-->

*html模板*

```
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="test.css">
	<title></title>
</head>
<body>
    <div id="a">
        <div id="b"></div>
    </div>
</body>
</html>
```

*CSS样式*

```
 #a {
     width:800px;
     height:800px;
 }
 #b {
     width:500px;
     height:500px
 }
```

# 方法一：绝对定位法

```
#a {
    position: relative;
}

#b {
    position:absolute; 
    /*#b 对于 #a 是绝对定位*/
    margin:auto; 
    /*浏览器计算margin的方式是左右的外边距等于父容器减去子容器剩余部分的宽度均匀分配给左右*/
    top: 0;left: 0;bottom: 0;right: 0; 
    /*top, left, bottom, right以包含margin的BOX设置块元素位置*/
}
```

优点：
1.响应式，自适应，可不固定`#a`, `#b`的高度
2.支持内容块重绘，支持resize:both


# 方法二：margin补偿法

```
#a {
    position: relative(absolute);
}

#b {
    position:absolute;
    left: 50%;
    top:50%;
    margin-left: -250px;  /*div#b宽度的一半*/
    margin-top: -250px;  /*div#b高度的一半*/
}
```

优点：
1.良好的跨浏览器特性，兼容IE6-IE7。
2.代码量少。

缺点：
1.不能自适应。不支持百分比尺寸和min-/max-属性设置。
2. 内容可能溢出容器。
3. 边距大小与padding,和是否定义box-sizing: border-box有关，计算需要根据不同情况。

# 方法三：transform法

```
#a {
    position: relative;
}

#b {
    position:absolute;
    left: 50%; /*向右偏移父容器的50%*/
    top:50%; /*向下偏移父容器的50%*/
    transform: translate(-50%,-50%); /*分别在x、y方向平移自身的50%*/
}
```

优点：
1.内容可变高度
2.代码量少

缺点：
1.IE8不支持
2.属性需要写浏览器厂商前缀
3.可能干扰其他transform效果
4.某些情形下会出现文本或元素边界渲染模糊的现象

# 方法四：table-cell法

*html模板*

```
    <div id="a">
        <div id="b">
            <div id="c"></div>
        </div>
    </div>

```
 
*css样式*

```
#a {
    height: 800px;
    width: 800px;
    background-color: #FF0000;
}

#c {
    height:500px;
    width:500px;
    background-color: #00FF00;
}

#a {
    display: table;
}

#b {
    display: table-cell;
    vertical-align: middle;
}
#c {
    margin:auto;
}
```

优点：
1.高度可变
2.内容溢出会将父元素撑开。
3.跨浏览器兼容性好。

缺点：
需要额外html标记

# 方法五：flexbox法

```
#a {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

优点：
1.内容块的宽高任意，优雅的溢出。
2.可用于更复杂高级的布局技术中。

缺点：
1.IE8/IE9不支持。
2.Body需要特定的容器和CSS样式。
3.运行于现代浏览器上的代码需要浏览器厂商前缀。
4.表现上可能会有一些问题

# 方法六：文字在div中垂直居中

```html
<body>
    <div id="a">
        <span id="b">womendeneirong</span>
        <span id="c"></span>
    </div>
</body>
```

```
#a {
    text-align: center;
    border: solid 3px #000000;
    width: 400px;
    height: 200px;
}

#b {
    vertical-align: middle;
}

#c {
    display: inline-block;
    height: 100%;
    vertical-align: middle;
}
```

参考文献
[盘点8种CSS实现垂直居中水平居中的绝对定位居中技术](http://blog.csdn.net/freshlover/article/details/11579669#)