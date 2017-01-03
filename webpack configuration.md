---
title: Webpack Configuration
tags: webpack
---

Webpack API Configuration

<!--more-->

# 前言
webpack 输入的是一个配置对象。有两种传递该配置对象的方法：CLI与node.js API，具体使用哪种取决于你使用webpack的方式。

## CLI
如果你使用的是CLI工具，可以读取到一个webpack.config.js(或者通过config选项传递文件)，文件应该输出配置对象如下：
```js
module.exports = {
    //configuration
}
```

## node.js API
如果你使用node.js API，你需要使用参数的方式传递配置对象：
```js
webpack({
    //configuration
}, callback)
```

## 多重配置
两种情况中你也可以使用一个配置数组，这样可以并行处理。他们共享文件系统缓存与监听，因此比多次调用webpack更有效

# 配置对象内容

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

## context——上下文

解析 `entry` 选项的绝对路径，如果 `output.pathinfo` 参数被设置，包含的pathinfo被该绝对路径缩短（即为绝对路径的子目录）

## entry——入口文件
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

## output——输出文件
影响编译输出的选项，`output` 选项告诉webpack如何将编译后的文件书写到磁盘中。注意，当有多重 `entry` 时，规定只有一个输出配置。
如果你使用任意的散列法（hash或者chunkhash），要确保考虑到了模块的尺寸，可以使用`OccurrenceOrderPlugin` 或者 `recordsPath`

### output.filename
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

### output.path
输出路径，必须为绝对路径
`hash` 被编译文件的hash值取代

### output.publicPath
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

### output.chunkFilename
没有入口的 chunks 文件名，其值是对于 `output.path` 的相对路径
[id] 是指 chunk 的id
[name] 是指 chunk 的名字（如果chunk没有名字则使用它的id）
[hash] 是指编译文件的hash值

### output.sourceMapFilename
JS文件的SourceMap文件名，这些文件保存在 output.path 目录

## module

影响常规模块的选项

### module.loaders

自动应用的加载器（loaders）数组，每一项都可以有如下一些属性：

- test 一定要满足的条件
- exclude 一定不满足的条件
- include 包含将被加载器转换的入口文件的路径或文件数组
- loader 用"!"分隔加载器的字符串
- loaders 加载器字符串数组

条件可以是一个正则表达式（用来测试绝对路径），可以是一个包含绝对路径的字符串，一个返回值为布尔值的函数，或者以上三种中一种的数组，（使用"and"连接）？？

详见[loaders官方文档](http://webpack.github.io/docs/loaders.html)或[loader中文文档](http://zhaoda.net/webpack-handbook/loader.html)

>注：这里的加载器被应用它们的相应的源码解析。这意味着它们不是被相关的配置文件解析。如果你使用npm安装加载器，并且你的node_modules文件夹没有包含全部的加载器或者不在父文件夹内，webpack将搜索不到加载器。你可以像resolveLoader.root选项中添加node_modules文件夹的绝对路径。`(resolveLoader: { root: path.join(__dirname, "node_modules") })`

### module.preLoaders, module.postLoaders



## resolve 

影响模块解析的选项

### resolve.alias 请求重定向

用其他路径或模块来替代当前模块，即把用户的一个require请求重定向到另一个路径
格式为对象，键名表示模块名，键值为新路径。这相当于做了一次替换，但是更智能。如果键值以$为结束符，只有当require请求中的参数与键值（不包含$）精确匹配时，模块才会被替换。
如果键值是相对路径，表示为发起require请求文件的相对路径

例子：在/abc/entry.js文件中发起require请求配置不同的重定向设置

alias | require("xyz") |  require("xyz/file.js")
----- | -------------- | -----------------------
{} | /abc/node_modules/xyz/index.js | /abc/node_modules/xyz/file.js
{ xyz: "/absolute/path/to/file.js" } | /absolute/path/to/file.js | error
{ xyz$: "/absolute/path/to/file.js" } | /absolute/path/to/file.js | /abc/node_modules/xyz/file.js
{ xyz: "/some/dir" } | /some/dir/index.js | /some/dir/file.js
{ xyz$: "/some/dir" } | /some/dir/index.js | /abc/node_modules/xyz/file.js
{ xyz: "xyz/dir" } | /abc/node_modules/xyz/dir/index.js | /abc/node_modules/xyz/dir/file.js
{ xyz$: "xyz/dir" } | /abc/node_modules/xyz/dir/index.js | /abc/node_modules/xyz/file.js

### resolve.root

容纳模块的目录**（绝对路径）**，也可以是一个目录数组。这项设置可以用来向搜索路径添加特殊目录。

```
var path = require('path');

// ...
resolve: {
  root: [
    path.resolve('./app/modules'),
    path.resolve('./vendor/modules')
  ]
}
```

### resolve.modulesDirectories

能够被当前目录及其祖先目录解析来搜索模块的目录名数组，这个方法与node寻找node_modules目录的方法相似。举例，如果值为["mydir"]，webpack会在“./mydir”, “../mydir”, “../../mydir”目录中查询

>注：默认为["web_modules", "node_modules"]

### resolve.fallback

当webpack没有在resolve.root或resolve.modulesDirectories中搜索到模块时，再去检索的一个目录（或绝对路径的目录数组）

### resolve.extensions

用来解析模块的扩展名数组，用于指明程序自动补全识别哪些后缀。举例，为了发现CoffeeScript文件，数组中应该包含字符串".coffee"。

>默认值：[".webpack.js", ".web.js", ".js"]

>注：设置该选项会重写默认值，意味着webpack不再会尝试解析默认扩展名文件的模块，如果你想要没有扩展名的模块能够正确解析，数组中必须包含一个空字符串。同样的，如果你想要不使用扩展名引用模块（例：require('underscore')）作为.js文件解析，你必须在数组中包含字符串".js"。

## resolveLoader

类似于resolve，为引用加载器（loaders）。

默认值：
```default
{
    modulesDirectories: ["web_loaders", "web_modules", "node_loaders", "node_modules"],
    extensions: ["", ".webpack-loader.js", ".web-loader.js", ".loader.js", ".js"],
    packageMains: ["webpackLoader", "webLoader", "loader", "main"]
}
```

>注：这里你可以使用alias或者resolve的其他特性，使用方法与resolve相似。如{ txt: 'raw-loader' }配置会使用raw-loader来解析txt!templates/demo.txt

### resolveLoader.moduleTemplates

该选项是resolveLoader的性能参数
描述的是loaders的命名规则和搜索优先顺序*不确定*

>Default: ["*-webpack-loader", "*-web-loader", "*-loader", "*"]

## externals

表示不应该被webpack打包的依赖，反而保留打包结果对它的引用。即使某些模块走CDN并以`<script>`的形式挂载到页面上来加载，但又希望能在 webpack 的模块中使用上。
（The requesting technique is modified to what is specified by output.libraryTarget or on a per dependency basis using a property-value pair object definition.）
因此，externals可以是如下的一个字符串，对象，函数，正则或数组。

## target

- web 编译结果可以在类浏览器环境中使用
- webworker 编译为WebWorker
- node 编译文件可以在类似nodejs环境中使用（使用require加载chunks）
- async-node 编译文件可以在类似nodejs环境中使用（使用fs和vm异步加载chunks）
- node-webkit
- electron
- electron-renderer

## bail

报告第一个错误为不可更正的错误而非容忍它

## profile

获取每个模块的时间信息

>提示：可以使用[分析工具](http://webpack.github.io/analyse/)将它可视化。`--json`和`stats.toJson()`可以将状态信息以JSON格式返回给你。

## cache










# 备注
>1. chunk 被entry所依赖的额外代码块

# 参考文献

[webpack中文网](https://webpack.vuefe.cn/concepts/index/)