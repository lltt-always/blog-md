---
title: Jquery的几种对象
tags: Javascript
---

详细介绍 Jquery 中几种对象——XMLHttpRequest, jqXHR, Promise Object 与 Deferred Object，及它们的联系与区别。

<!--more-->

# XMLHttpRequest

一些jQuery的Ajax函数返回原生XMLHttpRequest对象，或者把它作为参数传递给success/error/complete操作。但当传输机制不是XMLHttpRequest时（例如，一个JSONP请求脚本，返回一个脚本 tag 时），返回的不是XHR对象。

# jqXHR

从jQuery1.5开始，$.ajax()返回jqXHR对象，是浏览器原生XMLHttpRequest的超集，当传输机制不是是XMLHttpRequest时（例如，一个JSONP请求脚本，返回一个脚本 tag 时），jqXHR对象尽可能的模拟原生的XHR功能。

$.ajax()返回的jqXHR对象实现了 Promise 接口, 使它拥有了 Promise 的所有属性，方法和行为。

常用方法

- jqXHR.done(function(data, textStatus, jqXHR) {}): 参阅 Deferred.done()
  
  参数：data, textStatus, 和 jqXHR 对象

  textStatus的状态("success", "notmodified", "nocontent"，"error", "timeout", "abort", 或者 "parsererror")

- jqXHR.fail(function(jqXHR, textStatus, errorThrown) {}): 参阅 Deferred.fail()

  参数：jqXHR 对象, textStatus, 和 errorThrown

- jqXHR.always(): 参阅 Deferred.always()
- jqXHR.then(): 参阅 Deferred.then()

# Promise Object

为了不让外部程序可以直接访问可改变状态的 Deferred 对象，它提供了 Deferred 对象的部分方法——then, done, fail, always, pipe, progress, state 和 promise。

# Deferred Object

从jQuery1.5开始，Deferred 对象提供一个注册多个回调函数到自管理的回调函数队列中
，根据不同情况调用回调函数，并传递同步/异步函数的成功或失败的状态。

常用方法

- deferred.always()
- deferred.done()
- deferred.fail()
- deferred.isRejected()
- deferred.isResolved()
- deferred.notify()
- deferred.notifyWith()
- deferred.pipe()
- deferred.progress()
- deferred.reject()
- deferred.rejectWith()
- deferred.resolve()
- deferred.resolveWith()
- deferred.state()
- deferred.then()
- jQuery.Deferred()
- jQuery.when()
- .promise()
