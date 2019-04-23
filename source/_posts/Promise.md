---
title: Promise
date: 2019-04-23 17:26:46
tags:
---
promise对象用于异步操作，一共有三种状态
* pending 初始值
* fulfilled 操作成功
* rejected 操作失败  

promise还有两个方法
* resolve 将pending转为fulfilled
* reject 将pending转为rejected

当状态改变后，promise.then就会执行

<!--more--> 

## 创建一个promise
```bash
let promise = new Promise((reslove, reject) => {

})
promise.then(res => {

}).catch(err => {

})
```
除了new一个新的promise外还有另外两种快捷的创建方式
```bash
Promise.reslove(func)
Promise.reject(func)
```

## .all()
该方法用于将多个promise包装成一个新的promise
```bash
let p = Promise.all([p1,p2,p3])
p.then(res => {  
})
```
其中p1,p2,p3是同时开始，并行执行，当p1,p2,p3的都成功时，p才会成功，并且返回的res也是对应传入参数顺序的数组

## .race()
```bash
let p = Promise.race([p1,p2,p3])
p.then(res => {  
})
```
当p1,p2,p3中有一个执行完成时，res就是第一个执行完成的返回