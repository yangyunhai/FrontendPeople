## 面试官：ES6的语法特性，如何给一个不懂的人讲symbol，应用场景有哪些？
### 内容概要
* 前言
* Symbol基础 （老司机可跳过）
* 使用场景讨论
* 总结

### 前言
Symbol是ES6引入的一个新特性 —— 新到什么程度呢？ES5之前是没有任何办法可以模拟Symbol的

但是，我们日常开发工作中，直接使用到Symbol的场景似乎很少。我在网上搜了很多资料，对Symbol开始逐渐加深了理解，接下来就谈谈我的一些看法。


### Symbol 基础知识
symbol 是一种全新的基本数据类型 （primitive data type）

Symbol()函数会返回symbol类型的值，作为构造函数来说它并不完整，因为它不支持语法：
```js
new Symbol()   // Uncaught TypeError: Symbol is not a constructor
```
每个从Symbol()返回的symbol值都是唯一的，使用Symbol()创建新的symbol值，并用一个可选的字符串作为其描述 —— 描述相同的两个Symbol值依然是不同的
```js
const symbol1 = Symbol();
const symbol2 = Symbol(42);
const symbol3 = Symbol('foo'); //描述

console.log(typeof symbol1); // "symbol"

console.log(symbol2 === 42); //  false

console.log(symbol3.toString()); // "Symbol(foo)"

console.log(Symbol('foo') === Symbol('foo')); //  false
```
一个symbol值能作为对象属性的标识符 —— 这是该数据类型仅有的目的（划重点）
```js
const obj = {};
const  myPrivateMethod  = Symbol();
obj[myPrivateMethod] = function() {...};
```
当一个 symbol 类型的值在属性赋值语句中被用作标识符，该属性（像这个 symbol 一样）是匿名的, 并且是不可枚举的 —— 因为这个属性是不可枚举的，它不会在循环结构 for( ... in ...) 中作为成员出现，也因为这个属性是匿名的，它同样不会出现在 Object.getOwnPropertyNames() 的返回数组里。
这个属性可以通过创建时的原始 symbol 值访问到，或者通过遍历 Object.getOwnPropertySymbols() 返回的数组。
在上面的代码示例中，只有通过保存在变量 myPrivateMethod的值可以访问到对象属性（划重点）
```js
const obj = {};

const aProperty = Symbol("a");
obj[aProperty] = "a";
obj[Symbol.for("b")] = "b";
obj["c"] = "c";
obj.d = "d";

for (let i in obj) {
  console.log(i); // 输出 "c" 和 "d"
}


console.log(Object.getOwnPropertySymbols(obj)); // [ Symbol(a), Symbol(b) ]

console.log(obj[aProperty]); // a
console.log(obj[Symbol.for("b")]); // b
```



全局共享的Symbol

JavaScript 有个全局 symbol 注册表
```js
Symbol.for() 参数为字符串
```

使用给定的key搜索现有的symbol，如果找到则返回该symbol。否则将使用给定的key在全局symbol注册表中创建一个新的symbol
```js
Symbol.keyFor() 参数为Symbol
```

从全局symbol注册表中，为给定的symbol检索一个共享的key（字符串）
```js
const a = Symbol.for('foo');
const b = Symbol.for('foo');
console.log(a === b); //true

const aKey = Symbol.keyFor(a);
console.log(aKey); //foo

const isEqual = Symbol.keyFor(Symbol.for("tokenString")) === "tokenString";
console.log(isEqual); //true
```

Symbol 类具有一些静态属性，这类属性只有几个, 即所谓的众所周知的 symbol。

它们是在某些内置对象中找到的某些特定方法属性的 symbol。 暴露出这些 symbol 使得可以直接访问这些行为；这样的访问可能是有用的，例如在定义自定义类的时候。 普遍的 symbol 的例子有：“Symbol.hasInstance”用于 instanceof ，“Symbol.iterator”用于类似数组的对象，“Symbol.search”用于字符串对象

以Symbol.hasInstance为例：
```js
class MyClass {
  static [Symbol.hasInstance](lho) {
    return Array.isArray(lho);
  }
}

console.log([1,2,3] instanceof MyClass); // true
```
所有众所周知的 symbol 列表（本文不详细介绍，有兴趣的自行查阅文档）：
```js
Symbol.iterator

Symbol.asyncIterator

Symbol.match

Symbol.replace

Symbol.search

Symbol.split

Symbol.hasInstance

Symbol.isConcatSpreadable

Symbol.unscopables

Symbol.species

Symbol.toPrimitive

Symbol.toStringTag
```
（对这些众所周知的Symbol有点困惑？不要紧，本文后面会进一步阐述这些的使用场景）

### Symbol 的使用场景
从上面的 Symbol 基础知识，我们知道有且仅有 2 种途径可以创建 Symbol 值 —— Symbol() 函数 和 Symbol.for() 方法

使用 Symbol() 函数创建出来的 Symbol 是独一无二的（参数字符串对此毫不影响）
```js
for (let i = 0; i < 100; i++) {
  Symbol('test')
}
```
即 上面代码创建的100个Symbol值都互不相同

这种唯一性在某些定义常量的场景带来了极大的便利

#### 使用场景一：定义常量
假设你正在开发一个日志记录模块，你希望提供 DEBUG，INFO，WARN 三种级别的日志
```js
log.levels = {
    DEBUG: Symbol('debug'),
    INFO: Symbol('info'),
    WARN: Symbol('warn'),
};
log(log.levels.DEBUG, 'debug message');
log(log.levels.INFO, 'info message');
```
在上面代码中，其实我们并不关心 DEBUG，INFO，WARN 的值的意义，我们仅仅是想获得三个唯一的值作为标志而已

假如我们使用数字来代替上面的Symbol
```js
log.levels = {
    DEBUG: 1,
    INFO: 2,
    WARN: 3,
};
```
在新增一个 ERROR级别的时候，我们还要小心不能跟之前的值（1，2，3中的任意一个）相同；或者当我们手滑不小心，把 INFO 的值写成 1 时，DEBUG 和 INFO 的日志就混淆在一起了 —— 这些麻烦的根源在于 number 的值并不是独一无二的

（p.s. Symbol 在这边的作用有点类似于Java中 的 Enum ）



#### 使用场景二：在对象中存放自定义元数据
对象中的元数据可以理解为 —— 相对次要的，不影响对象内容的特殊属性。

对象的主要内容和属性是那些可以由 Object.getOwnProperties() 获取到的属性。

自定义元数据可以

给目标对象添加标记，以便做特殊处理
作为对象内部的一个“私有”属性，例：
```js
const size = Symbol('size');
class Collection {
  constructor() {
    this[size] = 0;
  }

  add(item) {
    this[this[size]] = item;
    this[size]++;
  }

  static sizeOf(instance) {
    return instance[size];
  }
}

const x = new Collection();
console.log(Collection.sizeOf(x) === 0); // true
x.add('foo');
console.log(Collection.sizeOf(x) === 1); // true
console.log(Object.keys(x)); //['0']
console.log(Object.getOwnPropertyNames(x)); // ['0']
console.log(Object.getOwnPropertySymbols(x)); // [ Symbol(size) ]
```
( 当然，上面这个例子完全可以使用 function 和 闭包实现真正的私有变量，并且我个人也推荐这么做；这边的代码仅做Symbol的一个使用例子）

提醒：由于Symbol对应的属性是不可遍历的，因此 JSON.stringfy 也不会碰这些元数据。


#### 使用场景三：在工具库开发中埋下hook（钩子函数）接入点
仍然以日志库为例，我们希望用户调用我们的 log 方法时可以自定义某些特殊对象的打印格式。
```js
import console from 'my-console-lib';
const inspect = console.Symbols.INSPECT; // 由我们的库导出的一个Symbol值
const myVeryOwnObject = {};
console.log(myVeryOwnObject); // 输出 `{}`
myVeryOwnObject[inspect] = function () { return 'DUUUDE'; }; // 用户可以给目标对象设置hook函数
console.log(myVeryOwnObject); // 输出 `DUUUDE` -- 优先执行用户的hook
```
>我们的 log 方法可以这么实现
```js
console.log = function (...items) {
  let output = '';
  for(const item of items) {
    if (typeof item[console.Symbols.INSPECT] === 'function') {
      output += item[console.Symbols.INSPECT](item);
    } else {
      output += console.inspect[typeof item](item);
    }
    output += '  ';
  }
  process.stdout.write(output + '\n');
}
```

#### 举个真实的例子：

在redux的源码中（github链接）
```js
  [Symbol.observable](): Observable<S>
  ```
使用了 Symbol.observable 这么一个 Symbol 作为 observable/reactive 库的接入口。

而 Symbol.observable 来自于一个规范 https://github.com/tc39/proposal-observable ，有兴趣的读者可以进一步研读。


#### 使用场景四：类似lodash的工具库
上面 Symbol基础章节中提到了“众所周知的Symbol”，它们通常可以用来定制对象的一些行为。

在日常开发中，其实我们很少能接触到这类需求。但是，在类似lodash这种工具库中，为了能做到广泛的适用性，就需要检查目标对象是否定义了这些特殊的Symbol，同时也要依据规范在返回的对象中设置好这些Symbol对应的属性。

#### 举个例子： lodash 的 toArray.js
```js
/** Built-in value references. */
const symIterator = Symbol.iterator // 众所周知的Symbol

function toArray(value) {
  if (!value) {
    return []
  }
  if (isArrayLike(value)) {
    return isString(value) ? stringToArray(value) : copyArray(value)
  }
  if (symIterator && value[symIterator]) { // 如果value包含这个Symbol，就利用这个Symbol遍历value的值
    return iteratorToArray(value[symIterator]())
  }
  const tag = getTag(value)
  const func = tag == mapTag ? mapToArray : (tag == setTag ? setToArray : values)

  return func(value)
}

export default toArray
```
除非你的工作项目就是这类库，否则对这些特殊的Symbol只需要简单了解就足够了（个人看法）。


#### 总结
Symbol 是 ES6 引入的一个全新的基础数据类型，它只拥有一些少量且简单的特性。

在合适的场景下，Symbol能发挥出更高效且灵活的作用。

欢迎分享你对Symbol的理解和使用场景，如果觉得这篇文章对你有帮助，记得一键三连！

### 参考资料
* https://zhuanlan.zhihu.com/p/183874695