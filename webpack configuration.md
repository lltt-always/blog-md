---
title: Webpack Configuration
tags: webpack
---

Webpack API Configuration

<!--more-->

#前言
webpack 输入的是一个配置对象。有两种传递该配置对象的方法：CLI与node.js API，具体使用哪种取决于你使用webpack的方式。

##CLI
如果你使用的是CLI工具，可以读取到一个webpack.config.js(或者通过config选项传递文件)，文件应该输出配置对象如下：
```js
module.exports = {
    //configuration
}
```

##node.js API
如果你使用node.js API，你需要使用参数的方式传递配置对象：
```js
webpack({
    //configuration
}, callback)
```

##多重配置
两种情况中你也可以使用一个配置数组，这样可以并行处理。他们共享文件系统缓存与监听，因此比多次调用webpack更有效

#配置对象内容

>请记住你不必在配置文件中书写纯JSON对象。可以使用任意你想用的Javascript。它只是一个node模板

一个简单的配置对象举例
```
{
    context: __dirname + "/app",
    entry: "./entry",
    output: {
        path: __dirname + "/dist",
        filename: "bundle.js"
    }
}
```

##context——上下文

解析 `entry` 选项的绝对路径，如果 `output.pathinfo` 参数被设置，包含的pathinfo被该绝对路径缩短（即为绝对路径的子目录）

##entry——入口文件
如果传递一个字符串：字符串被解析为开始时加载的模块。
如果传递一个数组：所有模块在开始时载入，最后一个被输出。

```
entry: ["./entry1", "./entry2"]
```

如果传递一个对象：会创建多个入口束，key为块名称，value可以为字符串或数组

```
{
    entry: {
        page1: "./page1",
        page2: ["./entry1", "./entry2"]
    },
    output: {
        // Make sure to use [name] or [id] in output.filename
        //  when using multiple entry points
        filename: "[name].bundle.js",
        chunkFilename: "[id].bundle.js"
    }
}
```

>注：不可以针对入口设置其他特别的配置，如果你需要对入口文件进行特殊配置，你需要使用上述的[多重配置](##多重配置)

##output——输出文件
影响编译输出的选项，`output` 选项告诉webpack如何将编译后的文件书写到磁盘中。注意，当有多重 `entry` 时，规定只有一个输出配置。
如果你使用任意的散列法（hash或者chunkhash），要确保考虑到了模块的尺寸，可以使用`OccurrenceOrderPlugin` 或者 `recordsPath`

###output.filename
指定每一个输出到磁盘的文件名。在这里绝对不能指定绝对路径。`output.path` 选项决定了文件写入磁盘的位置，`filename` 仅用作每个文件的命名

- 单入口

```
{
  entry: './src/app.js',
  output: {
    filename: 'bundle.js',
    path: __dirname + '/build'
  }
}

// writes to disk: ./build/bundle.js
```

- 多入口
如果你的配置创建了不止一个“chunk”块（比如使用了多入口或者使用了类似CommonsChunkPlugin插件），你应该使用“替代物”来确保每个文件的名字都不相同。
`name` 被chunk名取代
`hash` 被编译文件的hash值取代
`chunkhash` 被chunk的hash值取代

```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/build'
  }
}

// writes to disk: ./build/app.js, ./build/search.js
```

###output.path
输出路径，必须为绝对路径
`hash` 被编译文件的hash值取代

###output.publicPath
`publicPath` 指定输出文件在浏览器中引用时的公共URL地址，对于使用`<link>`、`<script>` 标签的加载或是像图片一样的引用资源，`publicPath` 被用作为文件的 href 或 url 当 publicPath 与他们在磁盘上的位置不同时（区别于 `path`）。这对于当你想要访问在一个不同域或者CDN的一些或者全部输出文件时很有帮助。Webpack Dev Server也使用这种方法来确定输出文件期望的服务器路径。和 `path` 相同，你可以使用 hash 优化缓存机制
- config.js
```
output: {
    path: "/home/proj/public/assets",
    publicPath: "/assets/"
}
```
- index.html
```
<head>
  <link href="/assets/spinner.gif"/>
</head>
```
- config.js
```
output: {
    path: "/home/proj/cdn/assets/[hash]",
    publicPath: "http://cdn.example.com/assets/[hash]/"
}
//使用CDN和hash值的例子
```

>注：如果在编译的时候还不知道最终输出文件的 `publicPath`，可以留为 blank 然后运行过程中在入口文件进行动态设置。如果在编译的时候你不知道 `publicPath` ，你可以省略它，然后在入口文件设置 `__webpack_public_path__`

```
 __webpack_public_path__ = myRuntimePublicPath

// rest of your application entry
```

#备注
>1. chunk 被entry所依赖的额外代码块