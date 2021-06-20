### 面试官：说说你对Js 事件循环机制的理解？
>我们都知道JavaScript是单线程语言,就是因为单线程的特性，就不得不提js中的同步和异步  

### **一、同步和异步**

所谓单线程，无非就是同步队列和异步队列，js代码是自上向下执行的，在主线程中立即执行的就是同步任务，比如简单的逻辑操作及函数，而异步任务不会立马立马执行，会挪步放到到异步队列中，比如ajax、promise、事件、计时器等等。

也就是先执行同步，主线程结束后再按照异步的顺序再次执行。

### **二、时间循环（Event Loop）**

Event Loop是什么？中文翻译是事件循环，等待主线程中任务全部完成后，再回来把异步队列中任务放到主程序中运行，这样反复的循环，就是事件循环。

![](![](https://files.mdnice.com/user/1099/29e68bdd-af37-4fd9-9d17-680e0bb234c0.jpg))  

先来看组代码

```js
console.log('开始111');
setTimeout(function() {
   console.log('setTimeout111');
}, 0);
Promise.resolve().then(function() {
   console.log('promise111');
}).then(function() {
   console.log('promise222');
});
console.log('开始222');
```


我们猜想一下上面的代码，会怎样打印？我们知道，肯定是先走同步的代码，从上往下，先打印 “开始111”，再打印“开始222”。  
中途的三个异步，进入到了异步队列，等待同步执行完（打印完），返回来再执行异步，所以是后打印出来。  
打印的结果先放一放，我们稍后回来再说。现在我们中途插播一段知识点：

### 三、宏观任务和微观任务（先执行微观任务，再执行宏观任务）

在事件循环中，每进行一次循环操作称为tick，tick 的任务处理模型是比较复杂的，里边有两个词：

分别是 Macro Task （宏任务）和 Micro Task（微任务）。

### 简单来说：

#### 宏观任务主要包含：
setTimeout、setInterval、script(整体代码)、I/O、UI 交互事件、setImmediate(Node.js 环境)

#### 微观任务主要包括：
Promise、MutaionObserver、process.nextTick(Node.js 环境)

#### 规范：
先执行微观任务，再执行宏观任务

那么我们知道了，Promise 属于微观任务， setTimeout、setInterval 属于宏观任务，先执行微观任务，等微观任务执行完，再执行宏观任务。所以我们再看一下这个代码：

```js
console.log('开始111');

setTimeout(function() {
  console.log('setTimeout111');
}, 0);

Promise.resolve().then(function() {
  console.log('promise111');
}).then(function() {
  console.log('promise222');
});

console.log('开始222');
```

#### 我们按照步骤来分析下：

* 1、遇到同步任务，直接先打印 “开始111”。  
* 2、遇到异步 setTimeout ，先放到队列中等待执行。  
* 3、遇到了 Promise ，放到等待队列中。  
* 4、遇到同步任务，直接打印 “开始222”。  
* 5、同步执行完，返回执行队列中的代码，从上往下执行，发现有宏观任务 setTimeout 和微观任务 Promise ，那么先执行微观任务，再执行宏观任务。

#### 所以打印的顺序为：
开始111 、开始222 、 promise111 、 promise222 、 setTimeout111 。

同理，我们再来分析一个代码：

```
console.log('开始111');

setTimeout(function () {
   console.log('timeout111');
});

new Promise(resolve => {
   console.log('promise111');
   resolve();
   setTimeout(() => console.log('timeout222'));
}).then(function () {
   console.log('promise222')
})

console.log('开始222');
```

#### 分析一下：

* 1、遇到同步代码，先打印 “开始111” 。  
* 2、遇到setTimeout异步，放入队列，等待执行 。  
* 3、中途遇到Promise函数，函数直接执行，打印 “promise111”。  
* 4、遇到setTimeout ，属于异步，放入队列，等待执行。  
* 5、遇到Promise的then等待成功返回，异步，放入队列。  
* 6、遇到同步，打印 “开始222”。  
* 7、执行完，返回，将异步队列中的代码，按顺序执行。有一个微观任务，then后的，所以打印 “promise222”，再执行两个宏观任务 “timeout111” “timeout222”。

#### 所以，打印的顺序为：
开始111 、 promise111 、 开始222 、 promise222 、 timeout111 、 timeout222 .

先执行主任务，把异步任务放入循环队列当中，等待主任务执行完，再执行队列中的异步任务。异步任务先执行微观任务，再执行宏观任务。一直这样循环，反复执行，就是事件循环机制。

### 参考资料
* https://www.cnblogs.com/tangjianqiang/p/13470363.html