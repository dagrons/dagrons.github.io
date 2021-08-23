<!--
.. title: js学习-理解promise机制
.. slug: jszhong-de-promiseji-zhi
.. date: 2021-03-23 11:18:42 UTC+08:00
.. tags: js
.. category: js 
.. link: 
.. description: 
.. type: text
-->

# Js中的核心机制-Promise

“在异步中实现同步”

如果说js中有什么东西造就了他的与众不同，我想，既不是functional programming这种几十年前的老东西，也不是None和undefined这种奇怪的机制，甚至于this的dynamic scope都不足以支撑js成为一门与众不同而独占前端市场的语言，而是他的异步机制，js是专为异步编程设计的语言，不只是体现在语法上，更重要的是其提供的基建。

在前端开发中，相当大的一部分工作是在进行UI渲染和回调处理，这不仅是web独有的特点，而是针对几乎所有前端开发的共有特性，在前端逻辑中，我们经常关注于一个按钮被点击后会发生什么，当我们从服务器中获取数据后要干什么...，于是有了“事件流”的概念，我们将事件流想象成一个队列，主线程从事件流中不断取出事件，并进行处理。

这里涉及两个问题，1. 主线程如何根据事件确定相应的处理逻辑？2. 主线程如何确保处理逻辑不会阻塞

对于问题1，js中提供了相当多的机制，典型的例子就是DOM的事件监听机制，用户在各种事件上注册各种处理函数，从而确定事件对应的处理逻辑，还例如fetch的回调，用于执行fetch返回后的处理逻辑

对于问题2，主线程如何确保处理逻辑不会阻塞，首先，我们要时刻谨记“js是单线程的”，也就是说，所有的操作都在一个线程内完成，这似乎很难理解，但回到我们之前的事件流模型，就不难发现，整个程序所做的事件不过是：

```javascript
while (q.size()) {
	event = q.pop_front();
	handle(event);
}
```

虽然js是单线程的，但浏览器并非单线程，浏览器只是为每个window开一个js线程，一个UI渲染引擎。

既然js是单线程的语言，所以要求我们的事件处理函数一定是不能阻塞我们的主线程，其中UI渲染事件会被交给UI渲染引擎进行处理，而fetch和setTimeout,setInterval，以及DOM的各种OnXXX事件，如何保证它们被处理的时候不阻塞呢，答案是：异步/non-blocking

在js中，异步/non-blocking常与Promise挂钩

## Promise

在js中，异步/non-blocking的含义就是将一个同步的过程分解成几个步骤，使得原来阻塞的处理逻辑变得不阻塞，举个例子：我们想要从server端获取数据，然后进行处理，但从server端获取数据的过程是会产生阻塞的，于是js提出了promise机制，首先它让fetch变成non-blocking的调用，然后允许我们对fetch的返回数据注册处理函数，但是这个处理函数是在fetch的返回数据确定后执行

所以，什么是Promise机制呢：

### Promise维护了操作的执行信息

用mdn上的话说：

> A Promise is an object representing the eventual completion or failure of an asynchronous operation.

也就是说Promise维护了异步操作执行情况的相关信息，pending, fullfilled, rejected

那除此之外呢？

### Promise提供了一套描述aysn_ops间执行关系的语言

Promise提供了一套Chain规则，让我们方便地描述异步操作的执行过程

如下：

```javascript
asyn_operations()
.then(successCallback, failureCallback)
.then(successCallback, failureCallback)
.then(...)
...
.catch(failureCallback)

注意：其中
1. .then((res)=>{}, null)简记为.then((res)=>{})
2. .then(null, (err)=>{})简记为.catch((err)=>{})
3. .then(res => {
	val = doSomething(res);
    return Promise.resolve(val); // returns a already resolved promise
    // likewise, Promise.reject(reason) returns a already rejected promise
})
简记为
    .then((res) => {
  	val = doSomething(res);
    return val;
})
所以：return val <=> return Promise.resolve(val) <=> return new Promise(resolve => resolve(val))
```

在ECMAScript 2017中，提供了更简洁的写法：（本质上是对上述写法的语法糖）

```javascript
async function foo() {
	try {
		const result = await async_operations();
		const result = await sync_or_async_ops(reuslt); 
		const result = await sync_or_async_ops(result); 
	}
	catch(error) {
		failureCallback(error);
	}
}
```

除了.then规则，还有.all和.any, .race...本质上都是对**各个操作间同步关系的描述**

例如：

```
Promise.all([fn1, fn2, fn3, fn4])
.then(([res1, res2, res3, res4]) => {
	do something
})

Promise.all()
```

### 那我们如何创建Promise呢

例如：

```javascript
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
```

上述定义了一个wait的函数，该函数接受一个ms参数，返回一个Promise对象，其中Promise维护了操作"resolve => setTimeout(resolve, ms)"的执行情况，

当我们使用wait时，即

```javascript
wait.then(doSomething) <=> new Promise(setTimeout(doSomething, ms))
```

从而创建了一个setTimeout的wrapper.



总的来说，Promise的主要贡献在于优化了异步回调中臃肿不堪的语法

## 注意事项

注意，Promise如果不进行异常捕获，如果执行过程出错，是不会造成程序终止的。
