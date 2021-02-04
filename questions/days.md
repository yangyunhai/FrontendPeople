#### 1.Vuex中有两个action，分别是actionA和actionB，其内都是异步操作，在actionB要提交actionA，需在actionA处理结束再处理其它操作，怎么实现？

如何利用`ES6`的`async`和`await`来实现。
```js
actions:{
    async actionA({commit}){
        //...
    },
    async actionB({dispatch}){
        //等待actionA完成
        await dispatch ('actionA')
        // ... 
    }
}
```

#### 2.在`v-model`上怎么用`Vuex`中`state`的值？
需要通过`computed`计算属性来转换。
```js
<input v-model="message">
// ...
computed: {
    message: {
        get () {
            return this.$store.state.message
        },
        set (value) {
            this.$store.commit('updateMessage', value)
        }
    }
}
```

### 3.如何让：
```
console.log('hello'.repeatify(3))
```
输出
```
hellohellohello
```

### 4.插入几万个 DOM，如何实现页面不卡顿？
### 方案一：
分页，懒加载，把数据分页，然后每次接受一定的数据，避免一次性接收太多
### 方案二：
* setInterval，
* setTimeout，
* requestAnimationFrame 

分批渲染，让数据在不同帧内去做渲染
### 方案三：
使用 virtual-scroll，虚拟滚动。
这种方式是指根据容器元素的高度以及列表项元素的高度来显示长列表数据中的某一个部分，而不是去完整地渲染长列表，以提高无限滚动的性能

### virtual-scroll原理：
在用户滚动时，改变列表在可视区域的渲染部分

* 计算当前可见区域起始数据的 startIndex
* 计算当前可见区域结束数据的 endIndex
* 计算当前可见区域的数据，并渲染到页面中
* 计算 startIndex 对应的数据在整个列表中的偏移位置 startOffset，并设置到列表上
* 计算 endIndex 对应的数据相对于可滚动区域最底部的偏移位置 endOffset，并设置到列表上

如下图所示：
![](https://npmhook.oss-cn-beijing.aliyuncs.com/2012012204_1606831485428.png)


startOffset 和 endOffset 会撑开容器元素的内容高度，让其可持续的滚动；此外，还能保持滚动条处于一个正确的位置。


### 5.你对 React 的 refs 有什么了解？

Refs 是 React 中引用的简写。它是一个有助于存储对特定的 React 元素或组件的引用的属性，它将由组件渲染配置函数返回。用于对 render() 返回的特定元素或组件的引用。当需要进行 DOM 测量或向组件添加方法时，它们会派上用场。
```js
class ReferenceDemo extends React.Component{
  display() {
      const name = this.inputDemo.value;
      document.getElementById('disp').innerHTML = name;
  }
  render() {
   return(        
      <div>
        Name: 
        <input type="text" ref={input => this.inputDemo = input}/>
        <button name="Click" onClick={this.display}>Click</button> 
        <h2>Hello <span id="disp"></span> !!!</h2>
      </div>
   );
  }
 }
```

### 6.Vuex的5个核心属性是什么？
分别是 state、getters、mutations、actions、modules 。

### 7.在Vuex中使用mutation要注意什么?
mutation 必须是同步函数


### 8.闭包的定义及其优缺点?
闭包就是函数嵌套函数，将函数作为函数的返回值返回出来，从而使全局拿到函数的变量，即f2作为f1的返回值返回出来，从而使全局能拿到f1的变量。

闭包出现的原因主要是因为，函数内部是可以拿到全局变量，但是全局是拿不到函数内部的函数的，为了解决这个问题，因为链式作用域的原因，函数嵌套的内层函数是可以拿到他父级函数的变量，所以将函数2作为函数1的返回值返回出去，从而使全局能够拿到函数1的变量。且该变量是不能被回收机制回收的，因为f1使f2的父函数，f2被赋予了一个全局变量，所以f2是始终在内存中，而f2依赖于f1存在，所以f1也始终在内存中，所以他们在调用结束不会被垃圾回收机制回收，

### 嵌套函数的闭包
```js
function aaa() {
    var a = 1;
    return function () {
        alert(a++)
    };
}
var fun = aaa();
fun();// 1 执行后 a++，，然后a还在~  
fun();// 2   
fun = null;//a被回收！！ 
```

闭包会使变量始终保存在内存中，如果不当使用会增大内存消耗。
### 优点：

1.可以在全局访问到函数内部的变量

2.变量始终在内存之中，不会在函数被调用后被清除

### 缺点：

1.占内存，因为所有闭包都是存在内存中的

2.在ie浏览器中国可能会导致内存泄漏，所以在函数推出前需要将没必要的函数删除。

### 09.什么是闭包及其作用域和原理?

闭包就是函数嵌套函数，将函数作为函数的返回值返回出来，从而使全局拿到函数的变量，即f2作为f1的返回值返回出来，从而使全局能拿到f1的变量。

闭包出现的原因主要是因为，函数内部是可以拿到全局变量，但是全局是拿不到函数内部的函数的，为了解决这个问题，因为链式作用域的原因，函数嵌套的内层函数是可以拿到他父级函数的变量，所以将函数2作为函数1的返回值返回出去，从而使全局能够拿到函数1的变量。且该变量是不能被回收机制回收的，因为f1使f2的父函数，f2被赋予了一个全局变量，所以f2是始终在内存中，而f2依赖于f1存在，所以f1也始终在内存中，所以他们在调用结束不会被垃圾回收机制回收，

### 优点：

1.可以在全局访问到函数内部的变量

2.变量始终在内存之中，不会在函数被调用后被清除

### 缺点：

1.占内存，因为所有闭包都是存在内存中的

2.在ie浏览器中国可能会导致内存泄漏，所以在函数推出前需要将没必要的函数删除。

### 10.javascript的垃圾回收原理?

在javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收；

如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。

### 11.什么叫原型以及原型链?

### 原型
​ 是一个可以被克隆的类，通过复制原型可以创建一个一模一样的新对象。

其实原型就相当于一个模板，它包含如下

1.它所有的引用类型都包括一个_ proto _(隐式原型)的属性，属性是一个普通的对象

2.所有构造函数都是有一个prototype（原型）属性，属性是一个普通的对象

3所有引用类型的_ proto 属性指向它的构造函数的prototype，即a为一个数组，a. _ proto_ === Array.prototype

### 原型链
​ 当访问一个对象的某个属性时，会先从该函数本身属性上找，如果没有找到，则会在他的 _ proto _隐式属性（即构造函数的prototype）上去找，如果还没有找到的话，则就会再去prototyep的 _ proto _ 中去找，这样一层一层向上查找就会形成一个链式结构，就称之为原型链。

构造器的实例“-proto-”属性指像的是构造器原型，所以构造原型上的属性和方法都能被实例访问到，加入A构造器的原型是B构造器的实例，B构造的原型是C构造器的实例，这样的话实例之间就形成一条由_“-proto-”_属性连接的原型链。

在原型链低端的实例可以使用原型链高端的属性和方法。

### 12.vue和react的共同点和不同点?

### 区别：

1.vue是双向数据绑定和react是单向数据绑定：

2.vue有指令，是指令编程，react没有指令，是函数式编程，以切皆函数。

3.vue是面向对象的高度体现，而react是利用es6的class类。

### 共同点：

1.都支持组件化

2.都是数据驱动视图

3.都是用虚拟dom实现快速渲染

react和vue的优缺点：

### vue的优点：

1.简单

2.异步处理方式更新DOM

3.耦合度不高，可以组件组合

4.对模块化友好，可以通过NPM等等安装，不强迫你所有的代码都遵循，使用场景更加灵活。

5.声明式渲染

### 缺点

不支持IE8

react的优点：

1.速度快，在UI渲染过程中，React通过在虚拟DOM中的微操作来实现对实际DOM的局部更新。

2.跨浏览器兼容，兼容IE8

### 缺点：

只是一个V层框架，开发大型项目，需要react-router+redux完成。


### 13.axios和ajax的区别?
### ajax：

1.AJAX不是新的编程语言，而是一种使用现有标准的新方法

2.最大优点是在不加载整个页面，可以与服务器交换数据并局部刷新网页内容。

3.不需要任何浏览器插件，但需要用户允许js在浏览器执行

### axios：

axios是通过promise实现对ajax技术的一种封装

1.用于浏览器和node.js的基于promise的HTTP的客户端

2.从浏览器制作XMLHttoRequests

3.让HTTP从node.js请求

4.支持promise API

5.拦截请求和相应（Interceptors拦截器）

6.转换请求和响应数据

7.取消请求

8.自动转换为JSON数据

9.客户端支持防止CSRF/XSRF（跨站请求伪造）

安卓4.43以下的手机还是不支持promise的，所以会报错，需要引入
```
npm install babel-polyfill
```
和
```
npm install babel-runtime
```
在入口文件上加上即可。

```js
import ‘babel-polyfill’
```

### 14.为什么要html语义化?

语义化HTML就是写出的HTML代码，符合内容的结构化（内容语义化），选择合适的标签（代码语义化），能够便于开发者阅读同时让浏览器的爬虫机器很好地解析。

1.有利于seo，提升网站的权重

2.在没有css的时候能够清晰看出网页的结构，增强可读性

3.便于团队开发和维护

4.支持多种端设备的浏览器渲染



### 15.为什么闭包不会被垃圾机制回收?

首先JS垃圾回收机制中，如果一个对象不再被引用，那么这个对象就会被垃圾机制回收，如果两个对象相互引用，而不在被第三方引用，那么这两个对象就会被垃圾回收机制回收。

因为闭包中，父函数被子函数引用，子函数又被外部所引用，这就是父函数不被回收的原因。

### 16.写出几种数组排序方式
### 冒泡排序
>遍历元素，跟其下一个元素对比
把最大的逐个往后排列
```
var arr = [12,3,44,343,55,1,23];
for(var i=0;i<arr.length-1;i++){
    for(var j=0;j<arr.length-i-1;j++){
        if(arr[j]>arr[j+1]){
            var current = arr[j];
            arr[j] = arr[j+1];
            arr[j+1] = current;
        }
    }
}
console.log(arr);
```
### 选择排序法
>把当前元素分别跟后面所有的元素对比
把最小的逐个往前排列
```js
var arr = [12,3,44,343,55,1,23];
for(var i=0;i<arr.length;i++){
    for(var j=i+1;j<arr.length;j++){
        if(arr[i]>arr[j]){
            var current = arr[i];
            arr[i] = arr[j];
            arr[j] = current;
        }
        console.log("666");
    }
}
console.log(arr);
```
### 快速排序法
```js
/*
* 利用递归函数实现排序
* 每次获取数组中间元素cItem
* 把大于和小于cItem的元素分别放置在arrGt和arrLt两个数组中,
* 利用concat组合递归调用函数返回的值
* 直到数组的长度等于1时，直接返回元素调出递归
*/
var arr = [10, 8, 20, 5, 6, 30, 11, 9]；
function fastSort(arr){
    //6. 递归退出条件
    if(arr.length<=1){
    	return arr;
    }
    //1. 找出数组中间位置元素
    var cIdx = parseInt(arr.length/2);

    //2.删除中间元素，避免与自己本身进行对比而造成死循环
    var cItem = arr.splice(cIdx,1);//[6],arr=[10, 8, 20, 5, 30, 11, 9]

    //3. 创建两个空数组用于保存大于或小于cItem的数字
    var arrLt = [];//[5]
    var arrGt = [];//[10,8,20,30,11,9]

    // 4.遍历数组，分别与cItem进行对比
    // 把小于cItem的数写入arrLt
    // 把大于cItem的数写入arrGt
    for(var i=0;i<arr.length;i++){
        if(arr[i]<cItem[0]){
        	arrLt.push(arr[i])
        }else{
        	arrGt.push(arr[i]);
        }
    }
    // 5.组合排序后的数组
    return fastSort([5]).concat(cItem,fastSort(arrGt));
}
console.log(fastSort(arr));
```

### sort排序
```js
arr.sort(function(a,b){
    // 在函数内通过返回值告诉sort方法如何排序
    return a-b;
});
```

### 17.什么是vue.mixin？
分发组件，可复用特别灵活的方式，汇入对象可以包含任意组件选项，所以当组件混入对象时，所有混入对象的选项将被混入该组件本身的选项。

简单来说，Vue.mixin()可以把你创建的自定义方法混入所有的 Vue的实例。


### 18.说说你们公司产品开放流程?
1.需求评审（时间线，开发计划书，功能列表，部署方案）

2.UI出设计图

3.前端开发用户界面逻辑+后端开发服务器逻辑

4.提交到测试服务器，出测试报告

5.改bug-提测-改bug

6.产品验收

7.提交到预发布服务器

8.上线

9.发上线报告

### 19.DNS是什么，解析域名的原理，过程是什么？

`DNS( Domain Name System)`是“域名系统”

DNS通过域名解析系统解析找到了对应的IP地址，

域

具体过程如下：

①用户主机上运行着DNS的客户端，就是我们的PC机或者手机客户端运行着DNS客户端了

②浏览器将接收到的url中抽取出域名字段，就是访问的主机名，比如

http://www.baidu.com/

③DNS客户机端向DNS服务器端发送一份查询报文，报文中包含着要访问的主机名字段（中间包括一些列缓存查询以及分布式DNS集群的工作）

④该DNS客户机最终会收到一份回答报文，其中包含有该主机名对应的IP地址

⑤一旦该浏览器收到来自DNS的IP地址，就可以向该IP地址定位的HTTP服务器发起TCP连接


### 20.如何处理页面的兼容性?
#### 1, ios/安卓混合开发有哪些兼容性问题
1. 多图上传问题: 安卓不能上传多张图片，没有什么解决方案
2. 浮动问题: 尽量用盒模型布局
3. 音频自动播放问题，ios默认不自动播放: 在document上添加点击事件播放音频
4. iOS横屏幕会重置字体大小: text-size-adjust: none;
5. i iOS手机页面里可滚动内容滚动不流畅:
6. 安卓浏览器看背景图片，有的会模糊 : background-size: contain；
7.  transition闪屏 transfrom-style:preserve-3d

##### 2, 浏览器问题；
##### 低版本常见问题: css
1. 当图片加 在 IE会出现边框；解决办法: border: 0 none；
2. 在div中插入图片，图片会在下边撑起三像素: 解决办法: img转换成块级元素；
3. 双倍浮动问题；给浮动元素添加margin会双倍显示；解决办法: 浮动元素加display: inline
4. 默认高度，低版本会出现默认高度: 解决办法: font-size: 0；
5. 表单元素行高不一致: 解决: vertical-align: middle；
6. 百分比BUG 50%+50%>100%，解决 给浮动元素添加标识 clear: right clear；left
7. 鼠标指针BUG cursor: hand 只能在IE9一下；cursor: point用于ie6以上高级版本
8. 透明属性opacity: 0-1 ie8: filter: alpha（opacity 0-100） ；
##### Css3 要加前缀
1. Chrome: -webkit-；
2. safari: -webkit- ;
3. opera: -o-;
4. firefox: -moz-；
5. ie: -ms-
##### JS兼容问题


| 问题 | 方法 |
| :-----| ----: |
| 获取样式(是获取, 不是设置) | getcomputedstyle（el） |
| window事件源	 | e = e |
| 键盘键码	 | e=e.which |
| 浏览器默认行为		 | event.preventDefault |
| 停止冒泡	 | event.stoppropagation |
| 事件监听	 |addEventListener|


### 21.什么是变量提升, ES6怎么避免?

变量提升, 也叫声明提前

使用var声明变量, 所用变量的声明都会提前到当前作用域的最开始执行 注意: 只有声明提前了, 赋值没有提前

ES6 中增加了let 和 const关键字用于声明变量

使用let和const声明的变量不会出现声明提前, 并且具有块级作用域


### 22.闭包面试题(以下代码输出什么)?
```js
function fun(n,o) {
  console.log(o)
  return {
    fun:function(m){
      return fun(m,n);
    }
  };
}
var a = fun(0);  
a.fun(1);  
a.fun(2);  
a.fun(3);
//undefined,?,?,?
var b = fun(0).fun(1).fun(2).fun(3);
//undefined,?,?,?
var c = fun(0).fun(1);  
c.fun(2);  
c.fun(3);
//undefined,?,?,?

//问:三行a,b,c的输出分别是什么？
```

### 解析:
这是一道非常典型的JS闭包问题。其中嵌套了三层fun函数，搞清楚每层fun的函数是那个fun函数尤为重要。

可以先在纸上或其他地方写下你认为的结果，然后展开看看正确答案是什么？

### 答案：
```js
//a: undefined,0,0,0
//b: undefined,0,1,2
//c: undefined,0,1,1
```
### 23.默认行为及阻止
浏览器以及`HTML`元素提供了一些默认行为，也可以称作默认事件。

## 默认行为

### a标签点击跳转
`<a>`标签在`href`存在的情况下会点击自动跳转链接或者定位锚点，通过对`<a>`的监听事件阻止默认行为后，点击链接不会跳转。

```html
<a href="https://blog.touchczy.top" id="t1">点击跳转链接</a>
<script type="text/javascript">
    document.getElementById("t1").addEventListener("click", (e) => {
        e.preventDefault();
    })
</script>
```

### 鼠标右击显示菜单
在浏览器页面中鼠标右击会显示菜单，通过对`document`的监听事件阻止默认行为后，右击页面不会弹出菜单，当然也可以通过监听并组织默认行为制作自定义右键菜单。

```html
<script type="text/javascript">
    document.addEventListener("contextmenu", (e) => {
        e.preventDefault();
    })
</script>
```

### input输入
在`<input>`或者`<textarea>`获得焦点时敲击键盘会自动输入，阻止默认行为后，敲击键盘将不会输入，可以在这个事件监听下作输入数据过滤，例如只允许输入数字。

```html
<input id="t3" />
<script type="text/javascript">
    document.getElementById("t3").addEventListener("keydown", (e) => {
        e.preventDefault();
    })
</script>
```

### 复选框选中
复选框的默认行为下是点击选中获取取消选中，阻止默认行为后，点击将不会改变目前状态。

```html
<input id="t4" type="checkbox" />
<script type="text/javascript">
    document.getElementById("t4").addEventListener("click", (e) => {
        e.preventDefault();
    })
</script>
```

### 表单提交
表单中若是存在`type`为`submit`的`<input>`或者是`<buttton>`都会触发表单的提交，阻止默认行为后表单不会自动提交。

```html
<form action="./" id="t5">
    <input type="submit" name="btn" />
</form>
<script type="text/javascript">
    document.getElementById("t5").addEventListener("submit", (e) => {
        e.preventDefault();
    })
</script>
```

## 阻止默认行为
* `W3C`推荐的阻止默认行为的方式是`event.preventDefault()`，此方法只会阻止默认行为而不会阻止事件的传播。
* `IE8`及之前的浏览器阻止默认行为需要使用`window.event.returnValue = false`。
* 直接在事件处理函数中`return false`也能阻止默认行为，只在`DOM0`级模型中有效。此外，在`jQuery`中使用`return false`会同时阻止默认行为与事件传播。

## 示例代码

```html
<!DOCTYPE html>
<html>
<head>
    <title>默认行为及阻止</title>
</head>
<body>
    <a href="https://blog.touchczy.top" id="t1">点击跳转链接</a>
    <input id="t3" />
    <input id="t4" type="checkbox" />

    <form action="/" id="t5">
        <input type="submit" name="btn" />
    </form>

    <a href="https://blog.touchczy.top" id="t6">点击跳转链接</a>

</body>
<script type="text/javascript">
    document.getElementById("t1").addEventListener("click", (e) => {
        e.preventDefault();
    })

    document.addEventListener("contextmenu", (e) => {
        e.preventDefault();
    })

    document.getElementById("t3").addEventListener("keydown", (e) => {
        e.preventDefault();
    })

    document.getElementById("t4").addEventListener("click", (e) => {
        e.preventDefault();
    })

    document.getElementById("t5").addEventListener("click", (e) => {
        e.preventDefault();
    })

    document.getElementById("t6").onclick = (e) => {
        return false;
    }
</script>
</html>
```

### 24.以下值输出什么？
```js
const hocCoBan = {};

Object.defineProperty(hocCoBan, "domain", {
    value: "hoccoban.com",    
})

async function App({year, age}){	
	return year - age + hocCoBan.domain.length;
}

App({year: 2021, age: 30}).then((r)=>{
  console.log(r)
});
```

### 答案区
* A: 2051
* B: 2001
* C: 30
* D: 2003

### 本题已收录于以下刷题小程序

### 答案
D

### 解答

上面的代码段似乎很复杂，涉及到我们如何利用Object.defineProperty向对象添加键和值的优势hocCoBan。

实际上，Object.defineProperty它具有几个方便的功能，这些功能使我们可以在某些情况下控制对象的行为，在这些情况下，我们要确保创建的对象是否可变，是否可迭代（使用for..in）等等。

例如，如果使用configurable: false声明对象时进行设置Object.defineProperty，则无法使用delete运算符删除该对象的属性。我们也不能更改该属性的值。

阅读上面的代码时，第二条“带走”消息是解包对象技术，或更常见的术语是破坏对象。假设您有一个带有两个称为year和的键的对象age，然后可以通过使用如下的销毁对象技术来获得它们：
```js
{year, age} = theOBject;
```
在上面的代码中，在声明函数时App，我们还使用销毁对象技术从对象中获取键并将它们用作参数。

如果您在使用关键字时熟悉JavaScript中的异步代码，async,那么了解为什么我们需要使用它then来App调用函数就没什么大不了的了。实际上，async总是返回一个承诺，因此我们需要使用then方法来获取所需的数据。

代码流为：
```js
2021-30 + "hoccoban.com".length（即12）。
```

>最终结果是2003。因此正确答案是D。