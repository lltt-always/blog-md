---
title: Webpack Configuration
tags: webpack
---

Webpack API Configuration

<!--more-->

webpack 输入的是一个配置对象。有两种传递该配置对象的方法：CLI与node.js API，具体使用哪种取决于你使用webpack的方式。

- CLI
如果你使用的是CLI工具，可以读取到一个webpack.config.js(或者通过config选项传递文件)，文件应该输出配置对象如下：
```js
module.exports = {
    //configuration
}
```

- node.js API
如果你使用node.js API，你需要使用参数的方式传递配置对象：
```js
webpack({
    //configuration
}, callback)
```

- 多重配置
两种情况中你也可以使用一个配置数组，这样可以并行处理。
