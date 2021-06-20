5.async await的理解

>async+await是es7提出来的概念，它也是为了解决回调地狱的问题，它只是一种语法糖.

从本质上讲，await函数仍然是promise，其原理跟Promise相似.

不过比起Promise之后用then方法来执行相关异步操作，async/await则把异步操作变得更像传统函数操作。

### async
async用于声明一个异步函数，该函数执行完之后返回一个 Promise 对象，可以使用 then 方法添加回调函数。请看下面代码:
```js
async function helloAsync(){
    return "helloAsync";
}
console.log(helloAsync());  // Promise {<resolved>: "helloAsync"}
helloAsync().then(v=>{
    console.log(v); // helloAsync
})
```
通过上面的代码可以得出结论，async 函数内return的值会被封装成一个Promise对象，由于async函数返回Promise对象，所以该函数可以按照Promise对象标准使用then方法进行后续异步操作。

如果要把async函数方法跟Promise对象方法做对比的话，那么下面的Promise对象异步方法代码是完全相等于上面的async函数异步方法。
```js
var helloAsync = function(){
    return new Promise(function(resolve){
        resolve("helloAsync");
    })
}
console.log(helloAsync())  
helloAsync().then(v=>{
    console.log(v);         
})
```
async函数运行的时候是同步运行的，Promise对象本身内容也是同步运行，这一点两者也是一致的，只有在then方法的时候才会被放入异步队列。

### await
await 操作符用于等待一个 Promise 对象，它只能在异步函数 async function 内部使用。

async函数运行的时候是同步运行，但是当async函数内部存在await操作符的时候，则会把await操作符标示的内容同步执行，await操作符标示的内容之后的代码则被放入异步队列等待。

（await标识的代码表示该代码运行需要一定的时间，所以后续的代码得进异步队列等待）

下面放一段await标准用法：
```js
function testAwait (x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}
 
async function helloAsync() {
  var x = await testAwait ("hello world");
  console.log(x); 
}
helloAsync ();
```
其实await多多少少对应了Promise对象异步方法里面的then方法，可以将上面代码改写成下面样式，结果也是一致的：
```js
function testAwait (x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}
 
async function helloAsync() {
  var x = testAwait ("hello world");//此处x是一个Promise对象
  x.then(function(value){
      console.log(value); 
  });
}
helloAsync ();
// hello world
```
上述方法把await去掉，使用then取代，能够起到同样的作用。两者都是把特定区域的代码放到异步队列中执行。

### 参考资料
* https://www.octgoodvps.com/archives/656
* https://blog.csdn.net/weixin_41615439/article/details/88896675    