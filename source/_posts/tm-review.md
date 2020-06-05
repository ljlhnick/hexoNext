---
title: TM codeview 知识回顾
categories: 
  - 前端
tags:
  - 工具
---
<!--more-->
## 只有一种情况返回bool值函数
这种情况可以直接用表达值返回，给字段设置bool值也可以直接用表达式

## 无依赖的请求可以用并行
不依赖其他请求的返回值，且对多个请求的数据进行统一的处理(es6结构)，可以直接用Promise.all([p1,p2]).then(result => {})

http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html

## Promise与async/await的区别
https://www.jianshu.com/p/fe0159f8beb4

Promise被明确拒绝时，会发生拒绝，但是若构造函数回调中引发错误，则会隐式拒绝。
例子
```
try {
    let p = new Promise((resolve, reject) => {
        throw new Error("I'm error");  // reject(new Error("I'm Error"));
    });
}catch(e) {
    console.log('catch',e);
}

let p = new Promise((resolve, reject) => {
    throw new Error("I'm error"); // reject(new Error("I'm Error"));
});
p.catch(e => {console.log('catch', e)});
```
async/await 寄生于Promise，Generator的语法糖。async函数返回Promise对象，可在被调用处使用then执行异步成功操作。但是Promise也可能会返回rejected，所以await命令放在try catch中。await强制把异步变同步，一句await执行完，才会执行下一句。

规则：async是异步函数，await只放在异步函数中。 await这等待promise返回结果，在继续执行

async错误处理： await不用写then，则catch也不用。直接用标准的try...catch语法捕捉错误。内部出错和上面Promise一样，不会在try...catch捕捉到。只会链式catch捕捉

## deferred对象
deferred为延迟对象，延迟到某个点才开始执行，改变执行结构的方法有两个(resolve、reject),分别对应两种回调(done、fail)，always不能成功失败都调用
ajax其实就是deferred对象
```
$.ajax(url, method).done(res => {}).fail(e => {})

//对个异步请求并行
$.when(ajax1, ajax2).done(resulit => {})
```
常用API
![deferred](https://images0.cnblogs.com/blog/453211/201308/17232146-f0b38ea700354dba9b68bdd2e550664b.jpg)

## 逻辑简化，善用map
```
this.allTechnicianList = [];
data.map(item => {
    this.allTechnicianList.push({
        id: item.Id,
        name: item.FullName
    })
})

this.allTechnicianList = data.map(item => { id: Item.id, name: item.name})
```

## eval用法
去掉对象外的""
```
eval('('+"{title : 'Something was wrong!',text:'Schedule start time must not be earlier than the current time.'}"+')')
```
