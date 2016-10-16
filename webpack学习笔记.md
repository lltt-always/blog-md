---
title: webpack-demos 学习笔记
tags: Webpack
---

简述webpack的功能与使用方法

<!--more-->

webpack 是一种模块化解决方案，它能把各种资源，如JS/JSX、coffee、CSS/LESS/SASS、图片等都作为模块来使用和处理。使用方式也简单，编写一个webpack.config.js配置文件，即可把有依赖关系的各种文件打包成一些列的静态资源。如果你的工程庞大，页面中使用了很多库，那就可以选择某种模块化解决方案，如 webpack、browserify。

## Webpack基础

### webpack.config.js 的基础结构

```javascript
module.exports = {
    entry: {
    }, //项目入口文件
    output: {
    }, //处理后的输出文件
    module: {
    }, //指定处理文件所需模块
    resolve: {
    }, //影响模块解析的选项
    externals: [
    ], //不需要被webpack解析的依赖
    plugins: [
    ] //添加外部编译插件
};
```

### 举个例子
- 首先，创建一个项目

`$ npm init`

- 安装 webpack 

`$ npm install webpack --save-dev`

- 定义项目目录
 - /app  
   main.jsx  
 - /build  
   index.html
 - package.json
 - webpack.config.js

- 编辑配置文件  

*webpack.config.js*
```javascript
var path = require('path');
var webpack = require('webpack');

module.exports = {
    entry: path.resolve(__dirname, './app/main.jsx'),
    output: {
        path: path.resolve(__dirname, './build'),
        filename: 'bundle.js',
    },
    module: {
        loaders: [{
            test: /\.jsx?$/,
            loader: 'babel',
            query: {
            presets: ['es2015', 'react']
            }
        }]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            }
        })
    ]
};
```

- Hello World页面实现  

*build/html*
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title>React Test</title>
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```

*app/main.jsx*
```
import React from "react";
import ReactDOM from "react-dom";

var Index = React.createClass({
    render: function() {
        return (
            <div>
                <h3>Hello World!!</h3>
            </div>
        );
    }
});

ReactDOM.render(
    <Index />,
    document.body
);
```

- 运行 webpack

`$ webpack`

- 打开 index.html  

### 配置 webpack-dev-server

上面的例子中简单实现了 webpack ，但是在开发过程中，如果修改代码需要不断运行 webpack 后再刷新页面，因此可以使用 webpack-dev-server 新建一个开发服务器，监听文件修改并自动重新打包代码。

- 使用 npm 安装 webpack-dev-server 

`$ npm install webpack-dev-server --save-dev`

- 运行 webpack-dev-server

`$ webpack-dev-server --content-base build //指定服务器打开目录`

- 浏览器打开 localhost:8080

### 配置浏览器自动刷新功能

上面的操作完成了webpack自动更新代码功能，为了提高工作效率，可以在命令行中中添加些指令使得代码更新的同时浏览器端也自动刷新。

`$ webpack-dev-server --inline --content-base build`  

打开localhost:8080，修改app文件夹下的jsx文件并保存的同时，浏览器也会自动刷新

为了方便输入，可以将指令添加到package.json的scripts字段中，

*package.json*
```
"scripts": {
    "dev": "webpack-dev-server --colors --inline --content-base build"
  }
```

`$ npm run dev`

关于 webpack-dev-server 的命令详情可参考[webpack-dev-server CLI](https://webpack.github.io/docs/webpack-dev-server.html)

## Webpack loaders

在 webpack.config.js文件中，module.loaders 模块用来分配加载器，以加载不同格式的文件。

### 安装

`$ npm install xxx-loader --save/--save-dev`

### 加载React文件

使用 babel loader 将jsx/es6格式的文件编译成js格式

*webpack.config.js*
```javascript
module.exports = {
    ...
    module: {
        loaders:[
            {
                test: /\.js[x]?$/,
                exclude: /node_modules/,
                loader: 'babel',  //注
                query: {
                    presets: ['es2015', 'react']
                }
            }
        ]
    }
    ...
};
```
babel-loader 的使用需要插件 babel-preset-es2015 和 babel-preset-react 的支持，在 babel-loader 的 query 选项中进行设置

如果注释行使用多个加载器如`loaders: ['style-loader', 'css-loader']`，则不要使用query参数，webpack编译器不能判断query属于哪一个loader。

### 加载CSS文件

webpack 允许你在js文件中引入CSS，然后使用CSS-loader来处理CSS文件 

*webpack.config.js*
```javascript
module.exports = {
  ...
    module: {
        loaders:[
            { 
                test: /\.css$/, 
                loader: 'style-loader!css-loader' 
                // Alternative syntax:
                loaders: ["style","css"]
            },
        ]
    },
  ...
};
```

为了载入CSS文件，需要两个加载器：css-loader & style-loader，前者用来读取CSS文件，后者用来将css样式插入到html页面，不同的加载器使用感叹号"!"来链接。

### 加载scss文件 & compass工具库

为了加载scss文件和compass工具库，需要加载raw-loader, style-loader, sass-loader
[参考资料](http://browniefed.com/blog/webpack-and-compass/)
```
{
    test: /\.scss$/, 
    loaders: ['style-loader', 'raw-loader', 'sass-loader']
}
```

### CSS Module

通过css modules，所有的css的classname和animation name都是本地scoped。webpack从一开始就加入了这个项目在css loader中，只是我们需要显示的开启它。有了这个功能，你可以export class来给指定的component。

*webpack.config.js*
```javascript
module.exports = {
    ...
    module: {
        loaders: [
            {
                test: /\.css$/,
                loader: 'style!css?modules'
            }
        ]
    }
    ...
}
```

*greeting.css*
```
.root {
  background-color: #eee,
  padding: 10px;
  border: 3px solid #ccc;
}
```

*greeting.js*
```
import React, {Component} from 'react'; 
import config from './config.json'; 
import styles from './Greeter.css';
class Greeter extends Component{ 
  render() {
    return (
      <div className={styles.root}>
      {config.greetText} </div>
    ); 
  }
}
export default Greeter
```

### Image loader  

webpack可以在js文件中引入images

*webpack.config.js*
```javascript
module.exports = {
    ...
    module: {
        loaders:[
            { 
                test: /\.(png|jpg)$/, 
                loader: 'url-loader?limit=8192' 
            },
        ]
    },
    ...
};
```
url-loader 用来载入图片文件，上面片段设置了 limit 参数，当图片小于8192字节时，将通过 Data URL的方式载入；当图片大于8192字节时，将通过 normal URL 的方式载入。

### More

更多详情可参考 [List of loaders](https://webpack.github.io/docs/list-of-loaders.html)

## Webpack plugin

插件可以为webpack增加一些特殊功能。内置的插件需要先引入webpack `require(webpack)` 再在 webpack config 中直接引用，没有内置的插件可以通过 npm 来安装，然后使用 require 引入。 

### 安装

`$ npm install xxx-plugin --save/--save-dev`


### UglifyJS Plugin

UglifyJS Plugin 可以将输出文件( bundle.js )最小化

*webpack.config.js*
```javascript
var webpack = require('webpack');
module.exports = {
  ...
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false // 编译过程中不使用 SourceMaps
            }
        })
    ],
  ...
};
```

### HTML Webpack Plugin and Open Browser Webpack Plugin  
其中 HTML Webpack Plugin 用来自动创建 index.html, Open Browser Webpack Plugin 可以在 webpack 加载时自动打开浏览器。

*webpack.config.js*
```javascript
var HtmlWebpackPlugin = require('html-webpack-plugin');
var OpenBrowserPlugin = require('open-browser-webpack-plugin');

module.exports = {
  ...
    plugins: [
        new HtmlwebpackPlugin({
            title: 'Webpack-demos'
        }),
        new OpenBrowserPlugin({
            url: 'http://localhost:8080'
        })
    ],
  ...
};
```
### CommonsChunkPlugin

当多个脚本包含相同的块时，可以使用CommonsChunkPlugin提取相同的部分到指定文件

*main1.jsx*
```
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(
    <h1>Hello World</h1>,
    document.getElementById('a')
);
```

*main2.jsx*
```
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(
    <h2>Hello Webpack</h2>,
    document.getElementById('b')
);
```

*webpack.config.js*
```javascript
var webpack = require('webpack');
module.exports = {
  ...
    plugins: [
        new webpack.optimize.CommonsChunkPlugin(init.js)
    ],
  ...
};
```

也可以用CommonsChunkPlugin提取指定的依赖库到指定文件  

*main.js*
```javascript
var $ = require('jquery');
$('h1').text('Hello World');
```

*webpack.config.js*
```javascript
var webpack = require('webpack');
module.exports = {
  ...
    plugins: [
        new webpack.optimize.CommonsChunkPlugin(/* chunkName= */'vendor', /* filename= */'vendor.js')
    ],
  ...
};
```

### ProvidePlugin

如果你希望在任意模块中使用一个模块作为变量，如 ‘jquery’ 而又不希望用 `require('jquery')` 来引入，这时可以使用 ProvidePlugin插件。

*main.js*
```javascript
$('h1').text('Hello World');
```

*webpack.config.js*
```javascript
var webpack = require('webpack');
module.exports = {
  ...
    plugins: [
        new webpack.ProvidePlugin({
        $: "jquery",
        jQuery: "jquery",
        "window.jQuery": "jquery"
    })
    ],
  ...
};
```

### More

更多详情可参考 [List of plugins](https://webpack.github.io/docs/list-of-plugins.html)

## Resolve
resolve 字段可以设置一些选项控制模块解析过程

`resolve.alias`——重定向依赖文件路径
`resolve.extensions`——用于指明程序自动补全识别哪些后缀
`resolve.modulesDirectories`——指定搜索解析模块的目录

## Externals

 exterals 字段用来设置不需要解析的外部依赖

*main.jsx*
```
var Index = React.createClass({
    render: function() {
        return (
            <div>
                <h3>Hello World!!!</h3>
            </div>
        );
    }
});

ReactDOM.render(
    <Index />,
    document.body
);
```

*webpack.config.js*
```
module.exports = {
    ...
    externals: {
        'react': 'React', //相当于 `import React from "react"` 且全局有效
        'react-dom': 'ReactDOM' //相当于 `import ReactDOM from "react-dom"`且全局有效
    },
    ...
};
```

*index.html*
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>React Test</title>
</head>
<body>
    <script type="text/javascript" src="../node_modules/react/dist/react.min.js"></script>
    <script type="text/javascript" src="../node_modules/react-dom/dist/react-dom.min.js"></script>
    <script src="bundle.js"></script>
</body>
</html>
```


## Environment flags
未调通
set DEBUG=true 不成功

## Code splitting

在大型前端项目中，将全部的代码放置在一个文件中会降低效率，webpack 允许把这些代码拆分为多个块。  
详情参考 [code splitting](http://webpack.github.io/docs/code-splitting.html)

使用 `require.ensure` 来定义分割点

*main.js*
```javascript
require.ensure(['./a'], function(require) {
    var content = require('./a');
    document.open();
    document.write('<h1>' + content + '</h1>');
    document.close();
});
```

*a.js*
```javascript
module.exports = 'Hello World';
```

webpack 可以理清依赖，输出文件和运行文件，因此不需要向 index.html 和 webpack.config.js 添加冗余信息。

### bundle-loader

用来分隔代码的另外一种方式，详情可参考 [bundle-loader](https://www.npmjs.com/package/bundle-loader)    

*main.js*
```javascript
var load = require('bundle-loader!./a.js');

load(function(file) {
    document.open();
    document.write('<h1>' + file + '</h1>');
    document.close();
});
```

## Command Line

- `--colors` 输出结果带颜色
- `--profile` 输出性能数据，查看每一步的耗时
- `--display-modules` 编译过程显示被隐藏模块


## References   

[webpack 官网](http://http://webpack.github.io/docs/)  
[webpack 中文指南](http://zhaoda.net/webpack-handbook/)  
[webpack-demos](https://github.com/ruanyf/webpack-demos)  
[webpack傻瓜式指南](https://zhuanlan.zhihu.com/p/20367175)  
[webpack 性能优化](http://code.oneapm.com/javascript/2015/07/07/webpack_performance_1/)







  

