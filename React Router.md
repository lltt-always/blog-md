---
title: React Router
tags: Javascript
---

简述React Router组件

<!--more-->

> Router 是一个组件，通过 render 函数引入来使用

[React Router API](https://github.com/reactjs/react-router/blob/master/docs/API.md#link)
[React Router 指导教程](https://github.com/reactjs/react-router-tutorial/tree/master/lessons)

# History
React Router 是建立在 history 上的。简单的说，一个history知道如何去监听浏览器地址栏的变化，将地址解析为路由器可以用来匹配路由的location对象，然后渲染正确的组件。
有三种常见的history，使用任意一种都可以为React Router的消耗自定义history工具。

## browserHistory
browserHistory 是为浏览器应用中的React Router推荐使用的history，它使用建立在浏览器中的history API来操作URL，创建真实的URL地址。
>使用

```
//Javascript module import
import { browserHistory } from 'react-router'
```

```
render(
  <Router history={browserHistory} routes={routes} />,
  document.getElementById('app')
)
```
## hashHistory

用url hash值和一个查询key来记录状态，因此地址栏的地址会有额外的垃圾 。hashHistory不需要额外的服务器配置，但是表现次于browserHistory。

## createMemoryHistory


    3. 在一个应用中，最常用的组件应该是<link>，它允许用户在应用中进行导航。它与<a>标签的唯一区别是它知道它的链接地址是否是活跃的。
        1. to：位置描述，通常是一个字符串或者对象，其语义如下,
            * 字符串：表示链接到的绝对地址（不支持相对地址）
            * 对象：
            * 
                1. pathname：一个字符串，表示链接到的地址
                2. query：待转换为字符串的key：value对象（弃用）
                3. hash：放入URL的hash值（弃用）
                4. state：保存location的状态（弃用）


        2. activeClassName：当<Link>路由被激活的className，默认没有active class
        3. activeStyle：当<Link>路由被激活的样式
        4. onClick(e)：点击事件的句柄函数，调用e.stopPropagation()可以阻止事件冒泡，调用e.preventDefault()可以组织开火的传递？？
        5. onlyActiveOnIndex：如果是true，当前路由和link路由匹配时被激活

    4. <Route>——配置组件。用来显式地映射路由到应用的组件层。
        1. path：在URL中使用的path，它可以是相对父路由的地址，但若以'/'开头则表示绝对地址。如果未定义，路由器会匹配子路由。注：绝对地址不能用于动态加载的路由配置。
        2. component：当路由与URL匹配时渲染的单一组件，它可以调用父路由组件的this.props.children属性来渲染
        3. components：当路径与URL匹配时，路由可以定义一个或多个named组件为[name]: component对象来渲染，可以嗲用父组件的this.props[name]

    5. 导航<Link>封装：一般情况下不需要知道<Link>是否被激活，但是主导航链接需要知道自己是否被激活，以显示正确的样式。



<IndexRedirect> 

允许对父路由重定向到其他路由，它可以设置成一个表示父路由默认路由的子路由。

```
<Router history={ browserHistory }>
    <Route name="home" path="/dockerkit/" component={ App }>
        <Route name="repo" path="repo/" component={ RepoList } />
        <Route name="testpage" path="testpage/" component={ Test } />
        <Route name="login" path="login/" component={ LoginPage } />
        <IndexRedirect from="*" to="/dockerkit/repo/" />
    </Route>            
</Router>
```

