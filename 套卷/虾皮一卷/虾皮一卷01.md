>js数据类型的检测方式，基本数据类型有哪些，null的数据类型是什么，（object),为什么是object?
## js数据类型的检测方式
### 1. typeof
typeof 是一个操作符，其右侧跟一个一元表达式，并返回这个表达式的数据类型。
返回的结果用该类型的字符串(全小写字母)形式表示

##### 包括以下 7 种：
number、boolean、symbol、string、object、undefined、function 等

typeof对于基本数据类型的判断是没有问题的，但是遇到了引用数据类型是不起作用的，只能返回一个object。（typeof(null)输出object 是因为在js中，null表示一个空对象指针）

* 对于基本类型，除 null 以外，均可以返回正确的结果。
* 对于 null ，返回 object 类型。
* 对于 function 返回 function 类型。

### 2、instanceof
instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。

在这里需要特别注意的是：instanceof 检测的是原型，我们用一段伪代码来模拟其内部执行过程：
```js
instanceof (A,B) = {
    var L = A.__proto__;
    var R = B.prototype;
    if(L === R) {
        // A的内部属性 __proto__ 指向 B 的原型对象
        return true;
    }
    return false;
}
 ```
3、constructor
先看一下用法：
```js
console.log(("1").constructor === String);
console.log((1).constructor === Number);
console.log((true).constructor === Boolean);
console.log(([]).constructor === Array);
console.log((function() {}).constructor === Function);
console.log(({}).constructor === Object);
```

输出结果：
```js
true
true
true
true
true
true
```

撇去null和undefined，似乎说constructor能用于检测js的基本类型和引用类型。但当涉及到原型和继承的时候，便出现了问题，如下：

```js
function fun() {};
fun.prototype = new Array();
let f = new fun();

console.log(f.constructor===fun);
console.log(f.constructor===Array);
```

撇去null和undefined，constructor能用于检测js的基本类型和引用类型，但当对象的原型更改之后，constructor便失效了。


从上述过程可以看出，当 A 的 proto 指向 B 的 prototype 时，就认为 A 就是 B 的实例



## 4.Object.prototype.toString.call()

用法：
```js
var test = Object.prototype.toString;
console.log(test.call("str"));
console.log(test.call(1));
console.log(test.call(true));
console.log(test.call(null));
console.log(test.call(undefined));
console.log(test.call([]));
console.log(test.call(function() {}));
console.log(test.call({}));
```
结果：
```js
[object String]
[object Number]
[object Boolean]
[object Null]
[object Undefined]
[object Array]
[object Function]
[object Object]
```
这样一看，似乎能满足js的所有数据类型，那我们看下继承之后是否能检测出来
```js
function fun() {};
fun.prototype = new Array();
let f = new fun();

console.log(Object.prototype.toString.call(fun))
console.log(Object.prototype.toString.call(f))
```
结果：
```js
[object Function]
[object Object]
```

可以看出，Object.prototype.toString.call()可用于检测js所有的数据类型。



### js基本数据类型
js有5种基本数据类型: number、boolean、undefined、null、string


### js null是什么类型?
null：是 Null类型，表示一个 空对象指针 或 尚未存在的对象

即该处不应该有值，使用typeof运算得到 object ，是个特殊对象值，转为数值为 0。

也可以理解是表示程序级的、正常的或在意料之中的值的空缺

作为函数的参数，表示该函数的参数不是对象

作为对象原型链的终点

#### 注意：
null 不是一个对象，但 typeof null === object 原因是不同的对象在底层都会表示为二进制，在 JS 中如果二进制的前三位都为 0，就会被判断为object类型，null 的二进制全为 0，自然前三位也是 0，所以 typeof null === ‘objcet’
##### undefined：
是Undefined 类型，表示一个 无 的原始值 或 缺少值，
即此处应该有一个值，但还没有定义，使用 typeof undefined === ‘undefined’，转为数值为 NaN。

它是在 ECMAScript 第三版引入的预定义全局变量，为了区分空指针对象 和 未初始化的变量。

也可以理解是表示系统级的、出乎意料的或类似错误的值的空缺

1. 变量被声但没有赋值时
2. 调用函数时，应该提供的参数没有提供时
3. 对象没有赋值的属性时，属性值为 undefined
4. 函数没有返回值时，默认返回值为 undefined


### （object),为什么是object
"()"也叫分组运算符,它有两种用法：如果表达式放在圆括号中，作用是求值；如果跟在函数后面，作用是调用函数,把表达式放在圆括号之中，将返回表达式的值
```js
console.log((1));  //1
console.log(('a')); //'a'
console.log((1+2)); // 3
```

把对象放在圆括号之中，则会返回对象的值，即对象本身
```js
var o = {p:1};
console.log((o));// Object {p: 1}
```

将函数放在圆括号中，会返回函数本身。如果圆括号紧跟在函数的后面，就表示调用函数，即对函数求值
```js
function f(){return 1;}
console.log((f));// function f(){return 1;}
console.log(f()); // 1
```

[注意]圆括号运算符不能为空，否则会报错
```js
();//SyntaxError: Unexpected token )
```

由于圆括号的作用是求值，如果将语句放在圆括号之中，就会报错，因为语句没有返回值
```js
console.log(var a = 1);// SyntaxError: Unexpected token var
console.log((var a = 1));// SyntaxError: Unexpected token var
```

### 参考资料
* https://segmentfault.com/a/1190000019259209
* https://blog.csdn.net/weixin_42878211/article/details/108037109
* http://blog.itblood.com/4590.html
* https://blog.csdn.net/weixin_34417200/article/details/88861212