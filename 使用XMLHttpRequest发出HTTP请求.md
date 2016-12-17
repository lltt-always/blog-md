---
title: 使用XMLHttpRequest发出HTTP请求
tags: Javascript
---

简述使用XMLHttpRequest发出HTTP请求的过程并举例，分析XMLHttpRequest对象的生命周期。

<!--more-->

# 代码结构
- 新建XMLHttpRequest实例
- 指定通信过程中状态改变的回调函数
- 指定HTTP请求方法，网址及是否异步
- 发送HTTP请求

# 代码示例

```
function httpRequset(success, failed) {
    let xhr = new XMLHttpRequest();
    xhr.open('GET', '/contactInf.json', true);
    xhr.onreadystatechange = function() {
        if(xhr.readyState === 4 && xhr.status === 200) {
            success(xhr.responseText);
        } else if(xhr.readyState === 4 && xhr.status !== 200) {
            failed(xhr.statusText);
        }
    };
    xhr.send(); 
}

function insertResp(resp) {
    document.getElementById('text').innerHTML = resp;
}

function typeError(error) {
    alert(error + ": Cannot get data");
}

httpRequset(insertResp, typeError);
document.getElementById('insert').innerHTML = '222';
```

# 一个成功请求过程中xhr的属性变化

| readyState | response | responseText | responseURL | status | statusText |
|:----------:|:--------:|:------------:|:-----------:|:------:|:----------:|
| 0          | ""       | ""           | ""          | 0      | ""         |
| 1          | ""       | ""           | ""          | 0      | ""         |
| 2          | ""       | ""           | URL         | 200    | "OK"       |
| 3          | 请求内容 | 请求内容     | URL         | 200    | "OK"       |
| 4          | 请求内容 | 请求内容     | URL         | 200    | "OK"       |

# readyState不同值的表示

- 0(创建对象)：(XMLHttpRequest)对象已经创建，但还没有调用open()方法。
- 1(初始化请求)：已经调用open() 方法，但尚未发送请求。
- 2(发送请求)：请求已经发送完成。
- 3(解析响应)：可以接收到部分响应数据。
- 4(完成)：已经接收到了全部数据，并且连接已经关闭。


