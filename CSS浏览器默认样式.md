---
title: CSS浏览器默认样式
tags: CSS
---

知己知彼才能百战百胜，为了设计出更完美的CSS样式，你需要知道的CSS浏览器默认样式。

<!--more-->

# user agent stylesheet

打开开发者工具的element面板，可以看到一些HTML元素的Styles来自user agent stylesheet，那么什么是user agent stylesheet呢？user agent stylesheet即为浏览器默认样式，可能一般在项目中为了实现自定义的CSS样式，你会首先使用CSS reset将浏览器默认设置清除，但是为了能灵活地掌握CSS，了解浏览器默认样式也是有必要的。

# 四大内核

- [Triden](http://www.iecss.com/)
- [Gecko](resource://gre-resources/)
- [Webkit](http://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css)
- [Blink](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css)

# W3C 标准

- [HTML4](https://www.w3.org/TR/CSS2/sample.html)
- [HTML5](https://www.w3.org/TR/html5/rendering.html)