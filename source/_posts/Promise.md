---
title: Promise
date: 2016-05-24 15:59:29
tags:
	- js
	- async
---

## Promise

> Promise 是什么

Promise 中文翻译是**许诺、约定**
也就是告诉后面的程序，等着吧，等哥执行完了，再给你结果。
本质和回调函数一样，不过写法好看多了，没有了回调黑洞（callback hell）
[关于Promise，可以看这里](http://www.cnblogs.com/lvdabao/p/es6-promise-1.html)
下面介绍一点用法

>	先看例子

```js
var p = new Promise(function(resolve, reject){
    setTimeout(function(){
        console.log('执行完成');
        resolve('哥执行完了,该你了');
    }, 2000);
});
p.then(function(data){
	console.log(data) //output:哥执行完了,该你了
});
```
Promise 包装并**执行**了一个函数，注意函数已经执行了，即使没有使用then
then 相当于回调函数作用，不影响函数执行
resolve和reject是两个参数，值会由Promise传递
<!-- more -->

> callback 写法

```js
function outer(callback){
    setTimeout(function(){
        console.log('执行完成');
        callback('哥执行完了,该你了');
    }, 2000);
};
outer(function(data){
	console.log(data) //output:哥执行完了,该你了
});
```
两者没什么不同，结果一样，这么看Promise貌似没啥用，
但是当回调多了呢？看下面的例子

>	开始正题 链式调用

```
runAsync1()
.then(function(data){
    console.log(data);
    return runAsync2();
})
.then(function(data){
    console.log(data);
    return runAsync3();
})
.then(function(data){
    console.log(data);
})
.then(function(){
    console.log("all is done")
})
.then(function(){
    console.log("all is done2")
});

function runAsync1(){
    return new Promise(function(resolve, reject){
        setTimeout(function(){
            console.log('异步任务1执行完成');
            resolve('随便什么数据1');
        }, 2000);
    });            
}
function runAsync2(){
    return new Promise(function(resolve, reject){
        setTimeout(function(){
            console.log('异步任务2执行完成');
            resolve('随便什么数据2');
        }, 2000);
    });            
}
function runAsync3(){
    return new Promise(function(resolve, reject){
        setTimeout(function(){
            console.log('异步任务3执行完成');
            resolve('随便什么数据3');
        }, 2000);
    });           
}
```

```
output:
异步任务1执行完成
随便什么数据1
异步任务2执行完成
随便什么数据2
异步任务3执行完成
随便什么数据3
all is done
all is done2
```
这时候还用callback?
层层回调代码不易读 ^_^
Promise尽管不好看，但是逻辑还是比较清晰的

> then 函数返回什么东西

	1，返回一个Promise对象
	2，return 任意值	（按照resolve处理）
	3，什么也不返回，也就是undefined（按照resolve处理）

> 出错了怎么办

	1,reject
    2,catch
两者区别，位置不一样，其他的貌似一样   
Promise和then包装的函数都有两个参数
```
new Promise(function(resolve, reject){
    //resolve or reject 二者必选其一
})

p.then(function(data){
    //resolve code
},function(){
	//reject    
}); //二者只能又一个执行
```
首先明确一个概念，Promise包装的函数不属于在reject,catch流程控制范围内（先提概念，下面会用到）
流程控制范围内的错误会在reject中处理，如果没有reject会在catch中处理,如果也没有catch，抱歉,老子不干了，
后面不管啦，但是函数也没有结束。
>	上例子吧

```
var  p = pro()
.then(function(data){
    console.log(data);
    console.log(a)
})
.then(function(){
    console.log("right")
},function(data){
    console.log(data);  //[ReferenceError: a is not defined]
});

function pro(){
    return new Promise(function(resolve, reject){    
        resolve('promise');
    });            
}
```
reject中处理
```
var  p = pro()
.then(function(data){
    console.log(data);
    console.log(a)
})
.then(function(data){
    console.log(data)
})

.catch(function(data){
    console.log("###" + data); // ###ReferenceError: a is not defined
});

function pro(){
    return new Promise(function(resolve, reject){    
        resolve('promise');
    });            
}```
catch处理

catch统一处理错误很方便的
 
> Promise 包装函数错了，是个啥情况 

```
var p = new Promise(function(resolve, reject){
    setTimeout(function(){
       a = b
    }, 2000);
});
p.then(function(){
},function(data){
    console.log(data)
})```
跟一般函数报错一样，自己试试吧

>   关于all,race 

都是并行执行的方法，增加些功能，语法都一样，就不讲了