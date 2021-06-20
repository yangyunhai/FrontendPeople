## 面试官：谈谈你对异步回调、promise、async/await的理解？
### promise的用法
Promise,简单来说就是一个容器，里面保存着某个未来才会结束的时间(通常是一个异步操作的结果)

Promise构造函数接收一个函数作为参数，该函数的两个参数是resolve，reject，它们由JavaScript引擎提供。其中resolve函数的作用是当Promise对象转移到成功,调用resolve并将操作结果作为其参数传递出去；

reject函数的作用是单Promise对象的状态变为失败时，将操作报出的错误作为其参数传递出去。


```js
 let p = new Promise((resolve,reject) => {
     resolve('success')
 });
```

### Promise的then方法

promise的then方法带有以下三个参数：成功回调，失败回调，前进回调，一般情况下只需要实现第一个，后面是可选的。Promise中最为重要的是状态，通过then的状态传递可以实现回调函数链式操作的实现

```js
 let p = new Promise((resolve,reject) => {
     resolve('success')
})
    
p.then(result => {
    console.log(result);
    //success
});

```

## Promise的其他方法
### catch用法
```js
function myPromise(res){
  return new Promise((resolve,reject)=>{
      if(res.data){
          resolve("成功数据");
      }else{
          reject("失败数据");
      }
  });
}

myPromise().then((message)=>{
    console.log(message);
  }
 )
.catch((message)=>{
    console.log(message);
})
```

这个时候catch执行的是和reject一样的，也就是说如果Promise的状态变为reject时，会被catch捕捉到，不过需要特别注意的是如果前面设置了reject方法的回调函数，·则catch不会捕捉到状态变为reject的情况。catch还有一点不同的是，如果在resolve或者reject发生错误的时候，会被catch捕捉到，这样就能避免程序卡死在回调函数中了。


### all用法

romise的all方法提供了并行执行异步操作的能力，在all中所有异步操作结束后才执行回调。
```js
function p1(){
    return new Promise((resolve,reject)=>{
        resolve("p1完成");
    })
}

function p2(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
           resolve("p2完成")
        },2000);
    })
}

function p3(){
    return new Promise((resolve,reject)=>{
        resolve("p3完成")
    });;
}

Promise.all([p1(),p2(),p3()])
.then((data)=>{
    console.log(data);
})
```

这里可以看到p2的resolve放到一个setTimeout中，最后的.then也会等到所有Promise完成状态的改变后才执行。


### race用法

在all中的回调函数中，等到所有的Promise都执行完，再来执行回调函数，race则不同它等到第一个Promise改变状态就开始执行回调函数。将上面的`all`改为`race`,得到
```js
Promise.race([p1(),p2(),p3()])
.then(function(data){
    console.log(data);
})
```

### async、await

异步编程的最高境界就是不关心它是否是异步。async、await很好的解决了这一点，将异步强行转换为同步处理。
async/await与promise不存在谁代替谁的说法，因为async/await是寄生于Promise，是Generater的语法糖。

### 用法

async用于申明一个function是异步的，而await可以认为是async wait的简写，等待一个异步方法执行完成。
规则：
* 1 async和await是配对使用的，await存在于async的内部。否则会报错
* 2 await表示在这里等待一个promise返回，再接下来执行
* 3 await后面跟着的应该是一个promise对象，（也可以不是，如果不是接下来也没什么意义了…）

### 写法：
```js
async function demo() {
 let result01 = await sleep(100);
 //上一个await执行之后才会执行下一句
 let result02 = await sleep(result01 + 100);
 let result03 = await sleep(result02 + 100);
 // console.log(result03);
  return result03;
}
```
```js
demo().then(result => {
    console.log(result);
});`

```

### 错误捕获
```js
let p = new Promise((resolve,reject) => {
    setTimeout(() => {
        reject('error');
    },1000);
});

async function demo(params) {
    try {
        let result = await p;
    }catch(e) {
       console.log(e);
    }
}

demo();
```

### 区别：
* 1 promise是ES6，async/await是ES7
* 2 async/await相对于promise来讲，写法更加优雅
* 3 reject状态：
 * 1）promise错误可以通过catch来捕捉，建议尾部捕获错误，
 * 2）async/await既可以用.then又可以用try-catch捕捉

### 参考资料
* cnblogs.com/ming1025/p/13092502.html
* blog.csdn.net/qq_37617413/article/details/90637694
* developer.mozilla.org