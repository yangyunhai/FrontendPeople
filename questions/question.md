#### 1.下面输出的结果为?
```javascript
function(){
    console.log(a)
    var a =1;
    function a(){}
    console.log(a)
}
```

A.`statements require a function name`

B.1,`ƒ a(){}`

C.`ƒ a(){}`,1

解析：

选  `A`


#### 2.下面输出的结果为?
```javascript
for(var i=0;i<3;i++){
    setTimeout(function(){
        console.log(i)
    },1000)
}
```

A.3

B.0,1,2,3

解析：

选  `A`


#### 3.下面输出的结果为?
```javascript
for(var i=0;i<10;i++){
    var a='a'
    let b = 'b'
}
console.log(a)
console.log(b)
```

A.a,`b is not defined`

B.a,b

解析：

选  `A`

上述代码中，变量`a`为全局变量


#### 4.下列输出结果为?
```javascript
let a = 100;
function test() {
  alert(a);
  a = 10;
  alert(a);
}
test();
alert(a);
```

A.100,10,10

B.100,100,10

解析：

选  `A`


#### 5.下列输出结果为?
```javascript
for(let i = 0; i <=3; i++){
	setTimeout(function(){
		console.log(i)
	}, 0)
}
```

A.0，1，2，3

B.3,3,3,3

解析：

选  `A`




#### 6.下列输出结果为?
```javascript
class People {
  constructor(name) {
    this.name = name;
    this.age = 20;
  }
  sayHi() {
    console.log(this);
  }
}
const zhangsan = new People("张三");
zhangsan.sayHi();

```

A.undefined

B.window

C.{"name":"张三","age":20}

解析：

选  `C`


#### 7.下列输出结果为?
```javascript
const zhangsan = {
  name: "张三",
  sayHi() {
    // this即当前对象
    console.log(this);
  },
  wait() {
    setTimeout(function () {
      // this === window
      console.log(this);
    });
  },
};

```

A.undefined

B.window

C.{"name":"张三"}

解析：

选  `A`




#### 8.下列输出结果为?
```javascript
function fn1(){
	console.log(this);
}
fn1.call({x: 100});
```

A.window

B.{x: 100}

解析：

选  `B`



#### 9.下列输出结果为?
```javascript
function fn1() {
  var a = {};
  Object.prototype = a;
  this.a = this;
  console.log(this);
}
fn1();
```

A.a

B.window

解析：

选  `B`




#### 10.下列输出结果为?
```javascript
function print(fn1) {
  let d = 200;
  fn1();
}
let d = 100;
function fn1() {
  console.log(d);
}
print(fn1);
```

A.100

B.200

解析：

选  `A`

自由变量的查找，是在函数定义的地方，向上级作用域中查找。不是在执行的地方！



#### 11.下列输出结果为?
```javascript
function create() {
  let b = 100;
  return function () {
    console.log(b);
  };
}
let fn = create();
let b = 200;
fn();
```

A.100

B.200

解析：

选  `A`




#### 12.下列输出结果为?

```javascript
var obj = {"key":"1","value":"2"};
var newObj = obj;
newObj.value += obj.key;
console.log(obj.value);
```
A.3

B.21

解析：

选  `B` 



#### 13.下列输出结果为?
```javascript

var m = 1,
  j = (k = 0);
function add(n) {
  return (n = n + 1);
}
y = add(m);
function add(n) {
  return (n = n + 3);
}
z = add(m);
console.log(y + "," + z);

```
A.0,4

B.1,4

C.4,4

解析：

选  `C` 


#### 14.下列输出结果为?

```javascript
var a = 1;
function Fn1() {
  var a = 2;
  console.log(this.a + a);
}

function Fn2() {
  var a = 10;
  Fn1();
}
Fn2();

var Fn3 = function () {
  this.a = 3;
};

Fn3.prototype = {
  a: 4,
};

var fn3 = new Fn3();
Fn1.call(fn3);
```

A.错误

B.3，3

C.2，10

D.3，5

解析：

选  `D` 



#### 15.`js`特性不包括（）?

A.解释性

B.用于客户端

C.基于对象

D.面向对象

解析：

选  `D` 


选 `D` 

#### 16.`js`中以下结果为`true`的是（）?

A.NaN==NaN

B.0==='0'

C.null==undefined

D.false=='0'

解析：

选 `C` `D` 

A:NaN == NaN  的执行结果是 false。因为JavaScript规定，NaN表示的是非数字，但是这个非数字也是不同的，因此 NaN 不等于 NaN，两个NaN永远不可能相等。

B:类型不一样

C:undefined的布尔值是false，null的布尔值也是false，所以它们在比较时都转化为了false，所以 undefined == null 。

D:值一样false=='0'，类型可以不一样



#### 17.下面哪个框架大量采用了组件化的开发模式（）?

A.`jquery`

B.`zepto`

C.`react`

D.`angular.js`

解析：

选`C`


#### 18.不属于`vue`的`props`验证类型的是哪一项（）?

A.`Function`

B.`Object`

C.`Map`

D.`Boolean`

解析：

选 `C` 

`props` `type` 可以是下列原生构造函数中的一个：

* String
* Number
* Boolean
* Array
* Object
* Date
* Function
* Symbol


##### 19.`Vue`中的`errorHandler`说法错误的是（）？

A.能捕获组件生命周期钩子里的错误

B.能捕获`Vue`自定义事件处理函数内部的错误

C.能捕获`v-on` `DOM`监听器内部抛出的错误

D.能捕获系统错误

选 `D` 


指定组件的渲染和观察期间未捕获错误的处理函数。这个处理函数被调用时，可获取错误信息和 `Vue` 实例

```javascript
Vue.config.errorHandler = function (err, vm, info) {
  #处理错误信息, 进行错误上报
  #err错误对象
  #vm Vue实例
  #`info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
  #只在 2.2.0+ 可用
}
```


#### 20.`Vue`的路由的传参方式正确的有：（）

A.
```js
this.$router.push({path:'',query：{}})
```

B.
```js
this.$route.push({path:'',params：{}})
```
C.
```js
this.$router.push({path：`/describe/${id}`})
```
D.
```js
this.$route.push({path：`/describe/${id}`})
```

解析：

选 `C` 

重点:`$router`  注意后面有`r`

// 字符串
```js
this.$router.push('/home/first')
```

// 对象
```js
this.$router.push({ path: '/home/first' })
```
// 命名的路由
```js
this.$router.push({ name: 'home', params: { userId: wise }})
```


#### 21.元素的过渡说法错误的是（）?

A.`v-enter-active`定义离开过渡生效时的状态

B.`v-leave-active`定义离开过渡生效时的状态

C.`v-leave`定义离开过渡的开始状态

D.`v-enter`定义进入过渡的开始状态

解析：

选 `A` 

`v-enter-active`：定义进入过渡生效时的状态。

在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。

这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

总的流程如下：

![](https://www.runoob.com/wp-content/uploads/2018/06/transition.png "")





##### 22.`Vue`中哪些属性可以写异步方法？

A.`getters`

B.`mutations`

C.`actions`

D.以上都不对

解析：

选  `C`

1.`state`：包含了store中存储的各个状态也叫数据。

2.`getter`: 类似于 Vue 中的计算属性，根据其他 `getter` 或 `state` 计算返回值。
//调用方法`store.getters.gettersFirst`

3.`mutation`: 一组方法，是改变store中状态的执行者。`Mutation` 必须是同步函数。 
//缓存的方法作用 调用方法例如： `store.commit('inc')`

4.`action`: 一组方法，其中可以含有异步操作。  
//调用方法 `store.dispatch('Actions')`



#### 23.`Vue`事件的修饰符（）？

A.`.stop`

B.`.native`

C.`.on`

D.`.enter`

解析：

选 `A` `B`

1)``.stop`：等同于`JavaScript`中的`event.stopPropagation()``，防止事件冒泡

2)`.prevent`：等同于`JavaScript`中的`event.preventDefault()`，防止执行预设的行为（如果事件可取消，则取消该事件，而不停止事件的进一步传播）

3)`.capture`：与事件冒泡的方向相反，事件捕获由外到内

4)`.self`：只会触发自己范围内的事件，不包含子元素

5)`.once`：只会触发一次

6)`.passive`：`passive`表示`listener`函数不会调用`preventDefault（）`

`passive`主要用在移动端的`scroll`事件，来提高浏览器响应速度，提升用户体验。因为`passive=true`等于提前告诉了浏览器，`touchstart`和`touchmove`不会阻止默认事件，手刚开始触摸，浏览器就可以立刻给与响应；

否则，手触摸屏幕了，但要等待`touchstart`和`touchmove`的结果，多了这一步，响应时间就长了，用户体验也就差了。



#### 24.`Vue`等单页面应用的优点（）？

A.不利于`SEO`

B.初次加载耗时相对增多

C.导航不可用，如果一定要导航需要自行实现前进、后退

D.具有桌面应用的即时性、网站的可移植性和可访问性

解析：

选 `D` 



#### 25.`Vue`的特性说法正确的是（）?

A.低耦合

B.可重用性

C.独立开发

D.以上均不对

解析：

选 `B` 



#### 26.`Vue`路由守卫函数是（）?

A.`beforeStart`

B.`beforeRun`

C.`beforeEach`

D.`beforeMap`

解析：

选 `C` 



#### 27.哪些属于`Vue`路由的模式?

A.`vuex`

B.`hash`
 
C.`history`
 
D.`map`

解析：

选`B` `C` 


#### 28.不属于`Vue`指令的是？（）

A.`v-if`

B.`v-template`

C.`v-model`

D.`v-html`

解析：

选`B` 



#### 29.对虚拟`dom`描述正确的是？（）

A.它直接用`JavaScript`实现了`DOM`树

B.组件的`HTML`结构并不会直接生成`DOM`，而是映射生成虚拟的`JavaScript` `DOM`结构

C.`React`又通过在这个虚拟`DOM`上实现了一个`diff`算法找出最小变更，再把这些变更写入实际的`DOM`中

D.这个虚拟`DOM`以`JS`结构的形式存在，计算性能会比较好，而且由于减少了实际`DOM`操作次数，性能会有较大提升

解析：

选`B` `C` `D`



#### 30.`react`的特点是什么？（）

A.数据双向绑定

B.`Virtual DOM`

C.组件化系统

D.双向数据流

解析：

选`B` `C`




#### 31.`react`中对于界面描述使用（）?

A.`html`

B.`xml`

C.`jsx`

D.`ejs`

解析：

选`C`



#### 32.`jsx`语法中不可以书写条件？

A.错误

B.正确

解析：

选`A`



#### 33.下面关于`React`描述正确的是（）?
A.使用虚拟`dom`

B.组件化开发

C.单项数据流

D.指令优化

解析：

选`A` `B` `C`



#### 34.`react`可以当成`mvc`中的v这一层?

A.错误

B.正确

解析：

选`B`



#### 35.`jsx`是一种`JavaScript`的语法扩展？

A.错误

B.正确

解析：

选`B`



#### 36.`react`实现了`angular`的内容可以写指令，服务，过滤器?

A.错误

B.正确

解析：

选`A`



#### 37.`react`中渲染元素使用`react-dom`包?

A.错误

B.正确

解析：

选`B`




#### 38.`Babel`转译器会把`JSX`转换成一个（）?

A.`html`结构

B.`css`

C.`es6`中的`class`

D.`React.createElement（）`的方法调用

解析：

选`D`




#### 39.`ReactDOM.render（）`方法中的第一个参数是（）?
A.选择器

B.`react`元素

C.`dom`节点

解析：

选`B`


##### 40、请选择下列代码的输出结果（ ）

```javascript
setTimeout (() => { 
    console.log(4)
})
new Promise (resolve => {
    resolve()
    console.log(1)
}).then (() => {
   console.log(3)
})
 console.log(2)
```

#### 答案

A、4-1-3-2

B、2-4-1-2

C、2-1-3-4

D、1-2-3-4

解析：

选`D`


`setTimeout`是宏任务来存在，而`Promise.then`则是具有代表性的微任务

所有会进入的异步都是指的事件回调中的那部分代码。

即`new Promise`在实例化的过程中所执行的代码都是同步进行的，而`then`中注册的回调才是异步执行的。
在同步代码执行完成后才回去检查是否有异步任务完成，并执行对应的回调，二位人物优惠在宏任务之前执行。




##### 41、`css`中`clear`的作用是什么？（ ）

A、清除该元素所有样式

B、清除该元素父元素的所有样式

C、指明该元素周围不可出现浮动元素

D、指明该元素的父元素周围不可出现浮动元素

解析：

选`C`

`clear` : `none` | `left` | `right` | `both`

对于`CSS`的清除浮动(`clear`)，
这个规则只能影响使用清除的元素本身，不能影响其他元素



##### 42.在 `css` 选择器当中，优先级排序正确的是（）

A、`id`选择器>标签选择器>类选择器

B、标签选择器>类选择器>`id`选择器

C、类选择器>标签选择器>`id`选择器

D、`id`选择器>类选择器>标签选择器


4个等级的定义如下：

第一等：代表内联样式，如: `style=””`，权值为`1000`

第二等：代表`ID`选择器，如：`#content`，权值为`100`

第三等：代表类，伪类和属性选择器，如`.content`，权值为`10`

第四等：代表类型选择器和伪元素选择器，如`div p`，权值为`1`



##### 43.下列定义的 `css` 中，哪个权重是最低的？（ ）

A、`#game .name`

B、`#game .name span`

C、`#game div`

D、`#game div.name`


权重越大，优先级越高

`CSS`选择器优先级是指`基础选择器`的优先级：

`ID` > `CLASS` > `ELEMENT` > `*`

##### 44、关于`HTML`语义化，以下哪个说法是正确的？（ ）

A、语义化的`HTML`有利于机器的阅读，如`PDA`手持设备、搜索引擎爬虫；但不利于人的阅读

B、`Table` 属于过时的标签，遇到数据列表时，需尽量使用 `div` 来模拟表格

C、语义化是`HTML5`带来的新概念，此前版本的`HTML`无法做到语义化

D、`header`、`article`、`address`都属于语义化明确的标签

解析：

选D

1、什么是`HTML`语义化？

根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。


##### 45、`CSS` 样式，下面哪一个元素能够达到最大宽度，且前后各有一个换行？（ ）

A、`Block Element`

B、`Square Element`

C、`Side Element`

D、`Box Element`

解析：

选A

块级元素`block element`

行内元素 `inline element`

行内块元素`inline-block element`


##### 46、放在`HTML`里的哪一部分`JavaScripts`会在页面加载的时候被执行？（ ）

A、文件头部位置

B、文件尾

C、`<head>`标签部分

D、`<body>`标签部分

解析：

选`D`

`head`部分中的`JavaScripts`会在被调用的时候才执行。

`body`部分中的`JavaScripts`会在页面加载的时候被执行。


##### 47、下列说法正确的有：（ ）

A、`visibility:hidden`;所占据的空间位置仍然存在，仅为视觉上的完全透明；

B、`display:none`;不为被隐藏的对象保留其物理空间；

C、`visibility:hidden`;与`display:none`;两者没有本质上的区别；

D、`visibility:hidden`;产生`reflow`和`repaint`（回流与重绘）；

选`A`、`B`

`visiblity`:看不见，摸的着.

`display`:看不见，摸不着.

`display`是`dom`级别的，可以渲染和重绘。

`visiblity`不是`dom`级别的，不能重绘，只能渲染

##### 48、新窗口打开网页，用到以下哪个值（ ）

A、`_self`

B、`_blank`

C、`_top`

D、`_parent`

解析：

选`B`

在`html`中通过`<a>`标签打开一个链接，通过 `<a>` 标签的 `target`

属性规定在何处打开链接文档。

如果在标签`<a>`中写入`target`属性，则浏览器会根据`target`的属性值去打开与其命名或名称相符的 框架`<frame>`或者窗口.

在`target`中还存在四个保留的属性值如下，

`_black`：在新窗口中打开被链接文档

`_self`：默认。在相同的框架中打开被链接文档

`_ parent`：在父框架中打开被链接文档

`_top`：在整个窗口中打开被链接文档

`framename`：在指定框架中打开被链接文档


##### 49、下列说法错误的是( )

A、设置`display:none`后的元素只会导致浏览器的重排而不会重绘

B、设置`visibility:hidde`后的元素只会导致浏览器重绘而不会重排

C、设置元素`opacity:0`之后，也可以触发点击事件

D、`visibility:hidden`的元素无法触发其点击事件

解析：

选`A`

设置`display:none`后的元素会导致浏览器的重排重绘


##### 50、超链接访问过后`hover`样式就不出现了，被点击访问过的超链接样式不再具有`hover`和`active`了，解决方法是改变`CSS`属性的排列顺序？（ ）

A、`a:link {}` `a:visited {}` `a:hover {}` `a:active {}`

B、`a:visited {}` `a:link {}` `a:hover {}` `a:active {}`

C、`a:active {}` `a:link {}` `a:hover {}` `a:visited {}`

D、`a:link {}` `a:active {}` `a:hover {}` `a:visited {}`

解析：

选`A`

`a:link`; `a:visited`; `a:hover`; `a:active;`

`a:hover`必须放在`a:link`和`a:visited`之后；

`a:active`必须放在`a:hover`之后。



##### 51、关于`position`定位，下列说法错误的是（ ）


A、`fixed`元素，可定位于相对于浏览器窗口的指定坐标，它始终是以 `body` 为依据

B、`relative`元素以它原来的位置为基准偏移，在其移动后，原来的位置不再占据空间

C、`absolute` 的元素，如果它的 父容器设置了 `position` 属性，并且 `position` 的属性值为 `absolute` 或者 `relative`，那么就会依据父容器进行偏移

D、`fixed` 属性的元素在标准流中不占位置

解析：

选`B`

`absolute`：
生成绝对定位的元素，相对于`static`定位以外的第一个父元素进行定位，元素的位置通过`left`、`top`、`right`、以及`bottom`属性进行规定
`fixed`：

生成绝对定位的元素，相对于浏览器窗口进行定位，元素的位置通过`left`、`top`、`right`以及`bottom`属性进行规定
`relative`：

生成相对定位的元素，相对于其正常位置进行定位，因此，`left：20`会向元素的`LEFT`位置添加20像素
`static`：

默认值，没有定位，元素出现正常的流中（忽略`top`，`bottom`，`left`，`right`或者`z-index`声明）
`inherit`：

规定应该从父元素继承`position`属性的值


###### 52、`css`中哪些属性可以继承（ ）

A、`font-size`

B、`color`

C、`font-family`

D、`border`

解析：

选`A`、`B`、`C`

`margin` `padding` `border` `display` 不可以继承