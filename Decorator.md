---
title: ES7 Decorator
tags: Javascript
---

介绍ES7 Decorator 的实现方法

<!--more-->

装饰器（Decorator）是一个函数，用来修改类的行为。装饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，装饰器能在编译阶段运行代码。

# 作用在类的方法上

格式如下：
```
function decoratorA(target, key, descriptor) {
    ...
}

class A {
    @decorator
    method(){};
}
```
其中
- target表示被装饰的方法所在类的prototype（如：A.prototype），是一个Object，包含类的属性与方法
- key表示被装饰的方法的键值（如：method），是一个String
- descriptor表示被装饰的方法的特性（对属性或方法的描述），是一个Object，包含writable（可读写），enumerable（可枚举），configurable（可配置）和value（函数体）四个特性

# 直接作用在类上

格式如下：
```
function decoratorB(target) {
    ...
}

@decoratorB
class B {}
```
其中，target表示被装饰的类（如：B）

若需要对不同类的decorator结果加以区别，可以对decorator传参，格式如下：
```
function decoratorC(arg) {
    return (target) => {
        ...
    }
}

@decoratorC(arg)
class C {}
```

# 一个实现了多种装饰方法的例子
[例子](https://github.com/lltt-always/Decorators)

- 装饰类方法的参数
- 装饰类方法的内容
- 装饰类的不可继承属性
- 装饰类的可继承属性的例子

# 参考目录

- [ES7 Decorator 装饰者模式](http://taobaofed.org/blog/2015/11/16/es7-decorator/)
- [Decorators in ES7](http://www.liuhaihua.cn/archives/115548.html)
- [【译】JavaScript设计模式：装饰者模式](http://www.codingserf.com/index.php/2015/05/javascript-design-patterns-decorator/)
- [ES6入门 修饰器](http://es6.ruanyifeng.com/#docs/decorator)
