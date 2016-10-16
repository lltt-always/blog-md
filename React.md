---
title: React
tags: Javascript
---

简述React常见问题

<!--more-->

React

- 允许在html模板中插入变量，{repo}
- super()
>如果有constructor函数，则一定要调用super()，否则this将为未定义的
- super()与super(props)
>区别在于你是否想在constructor函数中访问this.props
[React ES6 class constructor super()](http://cheng.logdown.com/posts/2016/03/26/683329)
- this.setState可以部分修改状态
- React key 用来标识组件的字段，不同的key值表示不同的组件
- 注释
```
var content = (
  <Nav>
    {/* 一般注释, 用 {} 包围 */}
    <Person
      /* 多
         行
         注释 */
      name={window.isLoggedIn ? window.name : ''} // 行尾注释
    />
  </Nav>
);
```



