---
title: JS继承机制原理（初级）
tags: Javascript
---

学习JS的继承机制

<!--more-->

- 创建构造函数

```
function human(gender, height, weight) {
    this.gender = gender;
    this.height = height;
    this.weight = weight;
}
```

- 生成实例对象

```
let man = new human('male', 180, 70);
```

- new指令的缺点

实例对象无法共享构造函数的属性与方法

```
function human(gender, height, weight) {
    this.gender = gender;
    this.height = height;
    this.weight = weight;
}

let man = new human('male', 180, 70);
let woman = new human('female', 160, 50);

console.log(man.weight); //70
console.log(woman.weight); //50

man.weight = 90;

console.log(man.weight); //90
console.log(woman.weight); //50
```

- 引入构造函数的prototype属性

prototype属性为一个对象，称prototype对象。
实例需要共享的属性、方法写入prototype对象；
实例不需要共享的属性、方法写入构造函数。
实例对象一旦创建自动应用prototype对象的属性与方法。

- 改变构造函数的prototype属性，由该构造函数生成实例的相应属性也跟随改变

```
function human(gender, height, weight) {
    this.gender = gender;
    this.height = height;
    this.weight = weight;
}

human.prototype = {
    legs: 2,
    arms: 2
};

console.log(man.legs); //2
console.log(woman.legs); //2

human.prototype.legs = 4;

console.log(man.legs); //4
console.log(woman.legs); //2
```

注：_proto_存放的是实例继承自构造函数prototype的属性与方法，可直接调用。