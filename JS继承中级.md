---
title: JS继承（中级）
tags: Javascript
---

JS继承的几种实现方法与差异比较

<!--more-->

### 约定

为了能够更好的比较不同继承方法的优缺点，我们对要被继承的父类做一个内容约定

```
// 统一的父类

function Fun() {
    // 私有属性
    var val = 1;   // 私有基本属性
    var arr = [];  // 私有引用属性
    function fun(){};  // 私有函数（引用属性）

    // 实例属性
    this.val = 1;   // 实例基本属性
    this.arr = [];  // 实例引用属性
    this.fun = function(){};  //  实例函数（引用属性）
}
// 原型属性
Fun.prototype.val = 1;  //原型基本属性
Fun.prototype.arr = [1];  //原型引用属性
Fun.prototype.fun = function(){};  //原型函数（引用属性）
```

### 方法一：构造函数法，父类实例充当子类的原型对象

```
function Father(){
    this.val = 1;
    this.arr = [1];
    this.fun = function(){};
}

function Son() {
    // ...
}

Son.prototype = new Father();
Son.prototype.constructor = Son;
```

优点：易于实现  
缺点：子类实例共享父类的引用属性，例：

```
var son1 = new Son();
var son2 = new Son();

son1.arr.push(2);

son1.arr; // [1, 2]
son2.arr; // [1, 2]
son1.arr == son2.arr; // true
```

### 方法二：构造函数法，借父类构造函数扩展子类实例

```
function Father(){
    this.val = 1;
    this.arr = [1];
    this.fun = function(){};
}

function Son() {
    // ...
    Father.call(this);
}
```

优点：解决了上一方法子类实例共享父类的引用属性问题  
缺点：无法实现函数复用，每个子类实例都持有一个新的function，易对性能造成影响，例

```
var son1 = new Son();
var son2 = new Son();

son1.arr.push(2);

son1.arr; // [1, 2]
son2.arr; // [1]
son1.arr == son2.arr; // false

son1.fun == son2.fun // false
```

### 方法三：组合法，组合上述两种方法

```
function Father(){
    // 只在此处声明基本属性和引用属性
    this.val = 1;
    this.arr = [1];
}
//  在此处声明函数
Father.prototype.fun1 = function(){};
Father.prototype.fun2 = function(){};

function Son(){
    Father.call(this);   // 核心
    // ...
}
Son.prototype = new Father();    // 核心
Son.prototype.constructor = Son;
```

优点：能够同时解决函数复用与不共享父类引用属性问题，  
缺点：（一点小瑕疵）子类原型上有一份多余的父类实例属性，因为父类构造函数被调用了两次，生成了两份，而子类实例上的那一份屏蔽了子类原型上的。又是内存浪费，比刚才情况好点，不过确实是瑕疵，例

```
var son1 = new Son(1);
var son2 = new Son(2);

son1.arr.push(2);
son1.arr; // [1, 2]
son2.arr; // [1]
son1.arr == son2.arr; // false

son1.fun == son2.fun; // true

son2;
/*
{
    arr: [1],
    val: 1,
    __proto__: {
        arr: [1],
        constructor: Son(),
        val: 1,
        __proto__: Object
    }
}
*/
```

### 方法四：寄生组合继承

```
function Father(){
    this.val = 1;
    this.arr = [1];
}

Father.prototype.fun1 = function(){};
Father.prototype.fun2 = function(){};

function beget(funAttr){
    function F(){};
    F.prototype = funAttr;
    return new F();
}

function Son(){
    // ...
    Father.call(this);
}

Son.prototype = beget(Father.prototype);
Son.prototype.constructor = Son;
```

优点：解决上述全部问题，  
缺点：写法略有复杂，例

```
var son1 = new Son();
var son2 = new Son();

son1.arr == son2.arr; // false
son1.fun1 == son2.fun1; //false

son1;
/*
{
    arr: [1],
    val: 1,
    __proto__: {
        constructor: Son(),
        __proto__: Object
    }
}
*/
```

### 方法五：Object.create()

```
function Father(){
    this.val = 1;
    this.arr = [1];
}

Father.prototype.fun = function(){};

function Son(){
    // ...
    Father.call(this);
}

Son.prototype = Object.create(Father.prototype); // 相当于上一方法中的beget；
Son.prototype.constructor = Son;
```

优点：完美解决上述问题，  
缺点：IE8不支持Object.create()，例

```
var son1 = new Son();
var son2 = new Son();

son1.arr == son2.arr; // false
son1.fun1 == son2.fun1; //false

son1;
/*
{
    arr: [1],
    val: 1,
    __proto__: {
        constructor: Son(),
        __proto__: Object
    }
}
*/
```





