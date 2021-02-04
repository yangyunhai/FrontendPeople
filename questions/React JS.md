#### 1.`React` 中 `keys` 的作用是什么？
>Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

```javascript
render () {
  return (
   ‹ul›
     {
      this.state.list.map(({item, key}) =› {
        return ‹li key={key}›{item}‹/li›
      })
     }
    ‹/ul›
  )
}
```

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 `React Diff` 算法中 `React` 会借助元素的 `Key` 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，`React` 还需要借助 `Key` 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 `Key` 的重要性。


#### 2.当你调用 `setState` 的时候，发生了什么事？

将传递给 `setState` 的对象合并到组件的当前状态，这将启动一个和解的过程，构建一个新的 `react` 元素树，与上一个元素树进行对比（ `diff` ），从而进行最小化的重渲染。

#### 3.为什么`setState` 的参数是一个 `callback` 而不是一个对

因为 `this.props` 和 `this.state` 的更新可能是异步的，不能依赖它们的值去计算下一个 `state`。

#### 4.状态(`state`)和属性(`props`)之间有何区别

`State` 是一种数据结构，用于组件挂载时所需数据的默认值。

`State` 可能会随着时间的推移而发生突变，但多数时候是作为用户事件行为的结果。

`Props`(properties 的简写)则是组件的配置。`props` 由父组件传递给子组件，并且就子组件而言，`props` 是不可变的(immutable)。

组件不能改变自身的 `props`，但是可以把其子组件的 props 放在一起(统一管理)。`Props` 也不仅仅是数据--回调函数也可以通过 props 传递。

#### 5.应该在 `React` 组件的何处发起 `Ajax` 请求?

在 `React` 组件中，应该在 `componentDidMount` 中发起网络请求。这个方法会在组件第一次“挂载”(被添加到 `DOM`)时执行，在组件的生命周期中仅会执行一次。

更重要的是，你不能保证在组件挂载之前 `Ajax` 请求已经完成，如果是这样，也就意味着你将尝试在一个未挂载的组件上调用 `setState`，这将不起作用。 在 `componentDidMount` 中发起网络请求将保证这有一个组件可以更新了。

#### 6.`React` 中的三种构建组件的方式？

`React.createClass()`、`ES6 class` 和无状态函数。

#### 7.`React` 中 `refs` 的作用是什么？

`Refs 是 `React` 提供给我们的安全访问 `DOM` 元素或者某个组件实例的句柄。

我们可以为元素添加 ref 属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回：

```javascript
class CustomForm extends Component {
  handleSubmit = () =› {
    console.log('Input Value: ', this.input.value);
  };
  render() {
    return (
      ‹form onSubmit={this.handleSubmit}›
        ‹input type='text' ref={input =› (this.input = input)} /›
        ‹button type='submit'›Submit‹/button›
      ‹/form›
    );
  }
}
```

上述代码中的 `input` 域包含了一个 `ref` 属性，该属性声明的回调函数会接收 `input` 对应的 `DOM` 元素，我们将其绑定到 this指针以便在其他的类函数中使用。

另外值得一提的是，`refs` 并不是类组件的专属，函数式组件同样能够利用闭包暂存其值：

```javascript
function CustomForm({ handleSubmit }) {
  let inputElement;
  return (
    ‹form onSubmit={() =› handleSubmit(inputElement.value)}›
      ‹input type='text' ref={input =› (inputElement = input)} /›
      ‹button type='submit'›Submit‹/button›
    ‹/form›
  );
}
```


##### 8.请描述下`react diff` 原理（常考，大厂必考）?

把树形结构按照层级分解，只比较同级元素。

给列表结构的每个单元添加唯一的 `key` 属性，方便比较。

`React` 只会匹配相同 `class` 的 component（这里面的 `class` 指的是组件的名字） 合并操作，调用 `component` 的 `setState` 方法的时候, `React` 将其标记为 `dirty`. 到每一个事件循环结束, `React` 检查所有标记 `dirty` 的 `component` 重新绘制. 选择性子树渲染。开发人员可以重写 `shouldComponentUpdate` 提高 `diff` 的性能。


#### 9.说说`React` 优势?

1、`React` 速度很快：它并不直接对 `DOM` 进行操作，引入了一个叫做虚拟 `DOM` 的概念，安插在 `javascript` 逻辑和实际的 `DOM` 之间，性能好。

2、跨浏览器兼容：虚拟 `DOM` 帮助我们解决了跨浏览器问题，它为我们提供了标准化的 `API`，甚至在 `IE8` 中都是没问题的。

3、一切都是 `component`：代码更加模块化，重用代码更容易，可维护性高。

4、单向数据流：`Flux` 是一个用于在 `JavaScript` 应用中创建单向数据层的架构，它随着 `React` 视图库的开发而被 Facebook 概念化。

5、同构、纯粹的 `javascript`：因为搜索引擎的爬虫程序依赖的是服务端响应而不是 `JavaScript` 的执行，预渲染你的应用有助于搜索引擎优化。

6、兼容性好：比如使用 `RequireJS` 来加载和打包，而 `Browserify` 和 `Webpack` 适用于构建大型应用。它们使得那些艰难的任务不再让人望而生畏。

#### 10.说说 `react` 生命周期函数?

### 初始化阶段：

`getDefaultProps`:获取实例的默认属性

`getInitialState`:获取每个实例的初始化状态

`componentWillMount`：组件即将被装载、渲染到页面上

`render`:组件在这里生成虚拟的 `DOM` 节点

`componentDidMount`:组件真正在被装载之后 运行中状态：

`componentWillReceiveProps`:组件将要接收到属性的时候调用

`shouldComponentUpdate`:组件接受到新属性或者新状态的时候（可以返回 `false`，接收数据后不更新，阻止 `render` 调用，后面的函数不会被继续执行了） `componentWillUpdate`:组件即将更新不能修改属性和状态

`render`:组件重新描绘

`componentDidUpdate`:组件已经更新

销毁阶段： 

`componentWillUnmount`:组件即将销毁

#### 11.`React`组件中怎么做事件代理理

虽然很多资料都说 `React` 的事件是会被代理到 `document` 上，但是我翻遍了官网，也没有找到相应的说明。

那么，有什么办法能够证明它吗？我想到了一个方法 → 通过 `Chrome` 浏览器的 `Event Listeners` 面板查看元素的绑定事件，具体的使用方法请参考官网文档。

![](https://user-images.githubusercontent.com/8401872/28256965-ac991142-6af9-11e7-844a-c6d72dc74666.png "")

从图中我们可以看到：

1.`#child` 元素绑定了两个点击事件，一个是通过 `React` 绑定的，一个是通过 `addEventListener` 绑定的。
2.通过 `addEventListener` 绑定的事件是真的绑定到 `#child` 元素上。
3.通过 `React` 绑定的事件，其实是代理绑定到 `document` 上。

#### React 模拟 DOM 事件冒泡机制

观察下面这个例子：`#child` 和 `#parent` 都绑定了一个点击事件。

![]([](https://user-images.githubusercontent.com/8401872/28257006-08cc350c-6afa-11e7-90a3-c32a055cd991.gif) "")


由图中可以看出：点击 `#child` 的同时，也触发了 `#parent` 的点击事件，看起来“很像” DOM 的事件冒泡机制。然而，实际原理并非如此，因为按照 `React` 的事件代理，`#child` 和 `#parent` 绑定的事件本来就是代理到 `document` 上的。也就是说，只有当事件流冒泡到 `document` 上时，才会依次触发 `document` 上绑定的两个事件。

到此为止，我以为我终于搞明白这块了，后来我发现我还是错了。如果说 `#child` 和 `#parent` 的事件都代理到 `document` 上的话，那么在 `Event Listeners` 面板中，我们应该能看到 2 个绑定在 `document` 上的事件，但实际上只有 1 个，如下图所示。

![]([](https://user-images.githubusercontent.com/8401872/28257139-0233191c-6afb-11e7-9144-c5507aa6a992.png) "")


因此，我们可以得出结论：并非 `#child` 和 `#parent` 的事件分别代理到 `document` 上，而是 `React` 在 `document` 上绑定了一个 `dispatchEvent` 函数（事件），在执行 `dispatchEvent` 的过程中，其内部会依次执行 `#child` 和 `#parent` 上绑定的事件。请注意，虽然 `dispatchEvent` 和代理到 `document` 上这两种方式的表现结果一样，但是其本质是有很大差别的，后边我们结合到 `stopImmediatePropagation` 的时候便会讲到。

那么这个 `dispatchEvent` 函数又是如何做到依次触发 `#child` 和 `#parent` 的事件的呢？我无力研究 `React` 这部分的源码，只好自己猜想了一下，其伪代码可能是这样子：

```JAVASCRIPT
 function dispatchEvent(event) {
     let target = event.target;
     target.click && target.click();  // 触发点击元素的事件
     while (target.parentNode) {      // 沿 DOM 向上回溯，遍历父节点，触发其 click 事件
         target.parentNode.click && target.parentNode.click();
         target = target.parentNode;
     }
 }
```

这应该便是 `React` 模拟 `DOM` 事件冒泡的大致原理。


#### React 禁止事件冒泡

既然有“事件冒泡”，就得有相应的禁止它的方法，这一点 `React` 的官网中便有提到：通过 `React` 绑定的事件，其回调函数中的 `event` 对象，是经过 `React` 合成的 `SyntheticEvent`，与原生的 DOM 事件的 event 不是一回事。准确地说，在 `React` 中，`e.nativeEvent` 才是原生 DOM 事件的那个 `event`，虽然 `React` 的合成事件对象也同样实现了 `stopPropagation` 接口。

因此，在 `React` 中，想要阻止“事件冒泡”（再强调一次，`React` 只是模拟事件冒泡，并非真正的 `DOM` 事件冒泡），只需要在回调函数中调用 `e.stopPropagation`。请注意，这时候的 `e.stopPropagation`非原生事件对象的 `stopPropagation`。

以上这些都是官网中已经有的，那本文又有什么新意呢？请看下面的例子：`#child`、`#parent` 和 `document` 上都绑定了事件，如何做到只触发 `#child` 上的事件？

![]([](https://user-images.githubusercontent.com/8401872/28257363-c2ffc11c-6afc-11e7-963d-3d391652db34.gif) "")


##### 我们来尝试解释一下上图中的现象：

事件流首先进入到 `#child` ，然后触发直接绑定在 `#child` 上的事件；

事件流沿着 DOM 结构向上冒泡到 `document`，触发 `React` 绑定的 `dispatchEvent` 函数，从而调用了 `#child` 子元素上绑定的 `clickChild` 方法。

在 `clickChild` 方法的最后，我调用了 `e.stopPropagation`，成功地阻止了 `React` 模拟的事件冒泡，因此，成功地没有触发 `#parent` 上的事件。

然后，最后出现了问题，还是触发了 document 上直接绑定的事件。我想要的是：”点击 `#child` ，只触发 `#child` 上的事件，不要触发任何其他元素的事件，包括 `document`，我应该怎么做呢？ → 答案是：调用`e.nativeEvent.stopImmediatePropagation`
上述过程用图解的方式来分析，我们能理解得清楚一些。

![]([](https://user-images.githubusercontent.com/8401872/28257456-8a4868b4-6afd-11e7-8cb0-8d35f272e27d.png) "")


`React` 合成事件对象的`e.stopPropagation`，只能阻止 `React` 模拟的事件冒泡，并不能阻止真实的 `DOM` 事件冒泡，更加不能阻止已经触发元素的多个事件的依次执行。在这种情况下，只有原生事件对象的 `stopImmediatePropagation`能做到。

你可能会说：”既然 `React` 在合成事件对象中封装了 `stopPropagation`，为什么不把
`stopImmediatePropagation` 也一并封装了呢？“

我的猜测是：”因为在 `React` 中不允许给同一个组件绑定多个相同类型的事件，如果非要重复绑定，那么后绑定的会覆盖前绑定的，这是它的设计思路。在这种设计思路下，不会存在某个组件有多个同类型的事件会依次触发，自然便不需要 `stopImmediatePropagation` 了。

### 总结
对于 `React` 的合成事件对象 `e` 来说：

`e.stopPropagation` → 用来阻止 React 模拟的事件冒泡

`e.stopImmediatePropagation` → 没有这个函数

`e.nativeEvent.stopPropagation` → 原生事件对象的用于阻止 `DOM` 事件的进一步捕获或者冒泡

`e.nativeEvent.stopImmediatePropagation` → 原生事件对象的用于阻止 `DOM` 事件的进一步捕获或者冒泡，且该元素的后续绑定的相同事件类型的事件也被一并阻止。


#### 12.`this` 的各种情况

call apply bind指的this是谁就是谁（bind不会调用，只会将当前的函数返回）

`fun.call(obj,a,b)` 

`fun.apply(obj,[  ])`

`fun.bind(obj,a,b)()`

#### this的情况：

1.以函数形式调用时，`this`永远都是window

2.以方法的形式调用时，`this`是调用方法的对象

3.以构造函数的形式调用时，`this`是新创建的那个对象

4.使用`call`和`apply`调用时，`this`是指定的那个对象

5.箭头函数：箭头函数的`this`看外层是否有函数

1）如果有，外层函数的`this`就是内部箭头函数的`this`
2）如果没有，就是`window`

6.特殊情况：通常意义上`this`指针指向为最后调用它的对象。这里需要注意的一点就是如果返回值是一个对象，那么`this`指向的就是那个返回的对象，如果返回值不是一个对象那么`this`还是指向函数的实例


##### 13.介绍`Promise`，异常捕获,如何进行异常处理?

`Promise`是解决回调地狱的好工具，比起直接使用回调函数`promise`的语法结构更加清晰，代码的可读性大大增加。

但是想要在真是的项目中恰当的运用`promise`可不是随便写个Demo这个简单的，如果运用不当反而会增加代码的复杂性。

使用`Promise`经常遇到的问题

##### 老旧浏览器没有Promise全局对象增么办?

可以使用`es6-promise-polyfill`。`es6-promise-polyfill`可以使用页面标签直接引入，当然也可以通过`es6`的`import`方法引入。

引入这个`polyfill`之后，它会在`window`对象中加入`Promise`对象。这样我们就可以全局使用`Promise`了。

##### 如何进行异常处理？

参照`promise`的文档我们可以在`reject`回调和`catch`中处理异常。但是`promise`规定如果一个错误在`reject`函数中被处理，那么`promise`将从异常常态中恢复过来。这意味着接下来的`the`n方法将接收到一个`resolve`回调。

大多数时候我们希望发生错误的时候，`promise`处理当前的异常并中断后续的`then`操作。
我们先来看一个使用`reject`处理异常的例子

```javascript
var promiseStart = new Promise(function(resolve, reject){
    reject('promise is rejected');
});

promiseStart.then(function(response) {
    console.log('resolved');
    return new Promise(function(resolve, reject){
        resolve('promise is resolved');
    });
})
.then(function (response){
    console.log('resolved:', response);
})
.catch(function(error) {
    console.log('catched:', error);
})
 ```
 
 ```js
  输出：
 catched: promise is rejected
 ```



 
#### 14.`React`怎么做数据的检查和变化

`props`:组件属性，专门用来连接父子组件间通信，父组件传输父类成员，子组件可以利用但不能编辑父类成员；

`state`：专门负责保存和改变组件内部的状态；

#### 数据传递

在`React`中,父组件给子组件传递数据时,通过给子组件设置`props`的方式,子组件取得`props`中的值,即可完成数据传递.被传递数据的格式可以是任何`js`可识别的数据结构

`props`一般只作为父组件给子组件传递数据用,不要试图去修改自己的`props`


#### 数据改变

`props`不能被自身修改,如果组建内部的属性发生变化使用`state`

```JAVASCRIPT
this.setState({ 
    ... 
})
```

`React`会实时监听每个组件的`props`和`state`的值,一旦有变化,会立刻更新组件,将结果重新渲染到页面上,`state`，`props`

 #### 15.介绍常见的`react`优化方式

 `React` 渲染性能优化的三个方向，其实也适用于其他软件开发领域，这三个方向分别是:

1.减少计算的量。 -> 对应到 `React` 中就是减少渲染的节点 或者 降低组件渲染的复杂度

2.利用缓存。-> 对应到 `React` 中就是如何避免重新渲染，利用函数式编程的 `memo` 方式来避免组件重新渲染

3.精确重新计算的范围。 对应到 React 中就是绑定组件和状态关系, 精确判断更新的'时机'和'范围'. 只重新渲染'脏'的组件，或者说降低渲染范围


 #### 目录

 #### 减少渲染的节点/降低渲染计算量(复杂度)
0️⃣ 不要在渲染函数都进行不必要的计算

1️⃣ 减少不必要的嵌套

2️⃣ 虚拟列表

3️⃣ 惰性渲染

4️⃣ 选择合适的样式方案

 #### 避免重新渲染
0️⃣ 简化 props

1️⃣ 不变的事件处理器

2️⃣ 不可变数据

3️⃣ 简化 state

4️⃣ 使用 recompose 精细化比对


 #### 精细化渲染
0️⃣ 响应式数据的精细化渲染

1️⃣ 不要滥用 Context
 #### 扩展

减少渲染的节点/降低渲染计算量(复杂度)
首先从计算的量上下功夫，减少节点渲染的数量或者降低渲染的计算量可以显著的提高组件渲染性能。


0️⃣ 不要在渲染函数都进行不必要的计算

比如不要在渲染函数(render)中进行数组排序、数据转换、订阅事件、创建事件处理器等等. 渲染函数中不应该放置太多副作用


1️⃣ 减少不必要的嵌套

所以我们需要理性地选择一些工具，比如使用原生的 CSS，减少 React 运行时的负担.

一般不必要的节点嵌套都是滥用高阶组件/RenderProps 导致的。所以还是那句话‘只有在必要时才使用 xxx’。 有很多种方式来代替高阶组件/RenderProps，例如优先使用 props、React Hooks

2️⃣ 虚拟列表

虚拟列表是常见的‘长列表'和'复杂组件树'优化方式，它优化的本质就是减少渲染的节点。

虚拟列表常用于以下组件场景:

无限滚动列表，grid, 表格，下拉列表，spreadsheets
无限切换的日历或轮播图
大数据量或无限嵌套的树
聊天窗，数据流(feed), 时间轴
等等

3️⃣ 惰性渲染

惰性渲染的初衷本质上和虚表一样，也就是说我们只在必要时才去渲染对应的节点。

举个典型的例子，我们常用 Tab 组件，我们没有必要一开始就将所有 Tab 的 panel 都渲染出来，而是等到该 Tab 被激活时才去惰性渲染。

还有很多场景会用到惰性渲染，例如树形选择器，模态弹窗，下拉列表，折叠组件等等。

这里就不举具体的代码例子了，留给读者去思考.

4️⃣ 选择合适的样式方案

![](https://pic2.zhimg.com/80/v2-119c336b929d57652d0ae7fcbb7bfa78_720w.jpg "")


所以在样式运行时性能方面大概可以总结为：`CSS` > `大部分CSS-in-js` > `inline style`


避免重新渲染
减少不必要的重新渲染也是 `React` 组件性能优化的重要方向. 为了避免不必要的组件重新渲染需要在做到两点:

保证组件纯粹性。即控制组件的副作用，如果组件有副作用则无法安全地缓存渲染结果
通过`shouldComponentUpdate`生命周期函数来比对 `state` 和 `props`, 确定是否要重新渲染。对于函数组件可以使用`React.memo`包装
另外这些措施也可以帮助你更容易地优化组件重新渲染:


0️⃣ 简化 props
① 如果一个组件的 props 太复杂一般意味着这个组件已经违背了‘单一职责’，首先应该尝试对组件进行拆解. ② 另外复杂的 props 也会变得难以维护, 比如会影响shallowCompare效率, 还会让组件的变动变得难以预测和调试.

下面是一个典型的例子, 为了判断列表项是否处于激活状态，这里传入了一个当前激活的 id:


这是一个非常糟糕的设计，一旦激活 id 变动，所有列表项都会重新刷新. 更好的解决办法是使用类似actived这样的布尔值 prop. actived 现在只有两种变动情况，也就是说激活 id 的变动，最多只有两个组件需要重新渲染.

简化的 props 更容易理解, 且可以提高组件缓存的命中率



1️⃣ 不变的事件处理器
①避免使用箭头函数形式的事件处理器, 例如:

<ComplexComponent onClick={evt => onClick(evt.id)} otherProps={values} />

假设 `ComplexComponent` 是一个复杂的 `PureComponent`, 这里使用箭头函数，其实每次渲染时都会创建一个新的事件处理器，这会导致 `ComplexComponent` 始终会被重新渲染.

更好的方式是使用实例方法:

```javascript
class MyComponent extends Component {
  render() {
    <ComplexComponent onClick={this.handleClick} otherProps={values} />;
  }
  handleClick = () => {
    /*...*/
  };
}
```

② 即使现在使用`hooks`，我依然会使用`useCallback`来包装事件处理器，尽量给下级组件暴露一个静态的函数:

```javascript
const handleClick = useCallback(() => {
  /*...*/
}, []);

return <ComplexComponent onClick={handleClick} otherProps={values} />;
```

但是如果`useCallback`依赖于很多状态，你的`useCallback`可能会变成这样:
```javascript
const handleClick = useCallback(() => {
  /*...*/
  //  
}, [foo, bar, baz, bazz, bazzzz]);
```

这种写法实在让人难以接受，这时候谁还管什么函数式非函数式的。我是这样处理的:

```javascript
function useRefProps<T>(props: T) {
  const ref = useRef < T > props;
  // 每次渲染更新props
  useEffect(() => {
    ref.current = props;
  });
}

function MyComp(props) {
  const propsRef = useRefProps(props);

  // 现在handleClick是始终不变的
  const handleClick = useCallback(() => {
    const { foo, bar, baz, bazz, bazzzz } = propsRef.current;
    // do something
  }, []);
}
```

③设计更方便处理的 `Event Props`. 有时候我们会被逼的不得不使用箭头函数来作为事件处理器：
```javascript
<List>
  {list.map(i => (
    <Item key={i.id} onClick={() => handleDelete(i.id)} value={i.value} />
  ))}
</List>
```

上面的 `onClick` 是一个糟糕的实现，它没有携带任何信息来标识事件来源，所以这里只能使用闭包形式，更好的设计可能是这样的:
```javascript
// onClick传递事件来源信息
const handleDelete = useCallback((id: string) => {
  /*删除操作*/
}, []);

return (
  <List>
    {list.map(i => (
      <Item key={i.id} id={i.id} onClick={handleDelete} value={i.value} />
    ))}
  </List>
);
```

如果是第三方组件或者 `DOM` 组件呢? 实在不行，看能不能传递`data-*`属性:
```javascript
const handleDelete = useCallback(event => {
  const id = event.dataset.id;
  /*删除操作*/
}, []);

return (
  <ul>
    {list.map(i => (
      <li key={i.id} data-id={i.id} onClick={handleDelete} value={i.value} />
    ))}
  </ul>
);
```


2️⃣ 不可变数据
不可变数据可以让状态变得可预测，也让 `shouldComponentUpdate` '浅比较'变得更可靠和高效.

相关的工具有`Immutable.js`、`Immer`、`immutability-helper` 以及 `seamless-immutable`。


3️⃣ 简化 state

不是所有状态都应该放在组件的 `state` 中. 例如缓存数据。按照我的原则是：如果需要组件响应它的变动, 或者需要渲染到视图中的数据才应该放到 `state` 中。这样可以避免不必要的数据变动导致组件重新渲染.



4️⃣ 使用 recompose 精细化比对

尽管 `hooks` 出来后，`recompose` 宣称不再更新了，但还是不影响我们使用 `recompose` 来控制`shouldComponentUpdate`方法, 比如它提供了以下方法来精细控制应该比较哪些 props:

```javascript
/* 相当于React.memo */
 pure()
 /* 自定义比较 */
 shouldUpdate(test: (props: Object, nextProps: Object) => boolean): HigherOrderComponent
 /* 只比较指定key */
 onlyUpdateForKeys( propKeys: Array<string>): HigherOrderComponent
 ```

其实还可以再扩展一下，比如`omitUpdateForKeys`忽略比对某些 `key`.


精细化渲染

所谓精细化渲染指的是只有一个数据来源导致组件重新渲染, 比如说 `A` 只依赖于 `a` 数据，那么只有在 `a` 数据变动时才渲染 `A`, 其他状态变化不应该影响组件 `A`。

`Vue` 和 `Mobx` 宣称自己性能好的一部分原因是它们的'响应式系统', 它允许我们定义一些‘响应式数据’，当这些响应数据变动时，依赖这些响应式数据视图就会重新渲染.来看看 Vue 官方是如何描述的:

![](https://picb.zhimg.com/80/v2-898d9a873ecbbdaed7be7d0e19a56099_720w.jpg "")



0️⃣ 响应式数据的精细化渲染
大部分情况下，响应式数据可以实现视图精细化的渲染, 但它还是不能避免开发者写出低效的程序. 本质上还是因为组件违背‘单一职责’.

举个例子，现在有一个 MyComponent 组件，依赖于 A、B、C 三个数据源，来构建一个 vdom 树。现在的问题是什么呢？现在只要 A、B、C 任意一个变动，那么 MyComponent 整个就会重新渲染:


更好的做法是让组件的职责更单一，精细化地依赖响应式数据，或者说对响应式数据进行‘隔离’. 如下图, A、B、C 都抽取各自的组件中了，现在 A 变动只会渲染 A 组件本身，而不会影响父组件和 B、C 组件:


举一个典型的例子，列表渲染:
```javascript
import React from 'react';
import { observable } from 'mobx';
import { observer } from 'mobx-react-lite';

const initialList = [];
for (let i = 0; i < 10; i++) {
  initialList.push({ id: i, name: `name-${i}`, value: 0 });
}

const store = observable({
  list: initialList,
});

export const List = observer(() => {
  const list = store.list;
  console.log('List渲染');
  return (
    <div className="list-container">
      <ul>
        {list.map((i, idx) => (
          <div className="list-item" key={i.id}>
            {/* 假设这是一个复杂的组件 */}
            {console.log('render', i.id)}
            <span className="list-item-name">{i.name} </span>
            <span className="list-item-value">{i.value} </span>
            <button
              className="list-item-increment"
              onClick={() => {
                i.value++;
                console.log('递增');
              }}>
              递增
            </button>
            <button
              className="list-item-increment"
              onClick={() => {
                if (idx < list.length - 1) {
                  console.log('移位');
                  let t = list[idx];
                  list[idx] = list[idx + 1];
                  list[idx + 1] = t;
                }
              }}>
              下移
            </button>
          </div>
        ))}
      </ul>
    </div>
  );
});
```

上述的例子是存在性能问题的，单个 `list-item` 的递增和移位都会导致整个列表的重新渲染:


原因大概能猜出来吧? 对于 Vue 或者 Mobx 来说，一个组件的渲染函数就是一个依赖收集的上下文。上面 List 组件渲染函数内'访问'了所有的列表项数据，那么 Vue 或 Mobx 就会认为你这个组件依赖于所有的列表项，这样就导致，只要任意一个列表项的属性值变动就会重新渲染整个 List 组件。

解决办法也很简单，就是将数据隔离抽取到单一职责的组件中。对于 Vue 或 Mobx 来说，越细粒度的组件，可以收获更高的性能优化效果:

```javascript
export const ListItem = observer(props => {
  const { item, onShiftDown } = props;
  return (
    <div className="list-item">
      {console.log('render', item.id)}
      {/* 假设这是一个复杂的组件 */}
      <span className="list-item-name">{item.name} </span>
      <span className="list-item-value">{item.value} </span>
      <button
        className="list-item-increment"
        onClick={() => {
          item.value++;
          console.log('递增');
        }}
      >
        递增
      </button>
      <button className="list-item-increment" onClick={() => onShiftDown(item)}>
        下移
      </button>
    </div>
  );
});
```

```javascript
export const List = observer(() => {
  const list = store.list;
  const handleShiftDown = useCallback(item => {
    const idx = list.findIndex(i => i.id === item.id);
    if (idx !== -1 && idx < list.length - 1) {
      console.log('移位');
      let t = list[idx];
      list[idx] = list[idx + 1];
      list[idx + 1] = t;
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  console.log('List 渲染');

  return (
    <div className="list-container">
      <ul>
        {list.map((i, idx) => (
          <ListItem key={i.id} item={i} onShiftDown={handleShiftDown} />
        ))}
      </ul>
    </div>
  );
});
```

效果很明显, list-item 递增只会重新渲染本身; 而移位只会重新渲染 List， 因为列表项没有变动, 所以下级 list-item 也不需要重新渲染


1️⃣ 不要滥用 Context
  
其实 Context 的用法和响应式数据正好相反。笔者也看过不少滥用 Context API 的例子, 说到底还是没有处理好‘状态的作用域问题’.

首先要理解 Context API 的更新特点，它是可以穿透React.memo或者shouldComponentUpdate的比对的，也就是说，一旦 Context 的 Value 变动，所有依赖该 Context 的组件会全部 forceUpdate.

这个和 Mobx 和 Vue 的响应式系统不同，Context API 并不能细粒度地检测哪些组件依赖哪些状态，所以说上节提到的‘精细化渲染’组件模式，在 Context 这里就成为了‘反模式’.

#### 总结一下使用 Context API 要遵循一下原则:

明确状态作用域, Context 只放置必要的，关键的，被大多数组件所共享的状态。比较典型的是鉴权状态

扩展：Context其实有个实验性或者说非公开的选项observedBits, 可以用于控制ContextConsumer是否需要更新. 详细可以看这篇文章<ObservedBits: React Context的秘密功能>. 不过不推荐在实际项目中使用，而且这个API也比较难用，不如直接上mobx。

粗粒度地订阅 Context
如下图. 细粒度的 Context 订阅会导致不必要的重新渲染, 所以这里推荐粗粒度的订阅. 比如在父级订阅 Context，然后再通过 props 传递给下级。

另外程墨 Morgan 在避免 React Context 导致的重复渲染一文中也提到 ContextAPI 的一个陷阱:
```javascript
<Context.Provider
  value={{ theme: this.state.theme, switchTheme: this.switchTheme }}>
  <div className="App">
    <Header />
    <Content />
  </div>
</Context.Provider>
```
上面的组件会在 state 变化时重新渲染整个组件树，至于为什么留给读者去思考。

所以我们一般都不会裸露地使用 Context.Provider, 而是封装为独立的 Provider 组件:
```javascript
export function ThemeProvider(props) {
  const [theme, switchTheme] = useState(redTheme);
  return (
    <Context.Provider value={{ theme, switchTheme }}>
      {props.children}
    </Context.Provider>
  );
}
```

// 顺便暴露useTheme, 让外部必须直接使用Context
```javascript
export function useTheme() {
  return useContext(Context);
}
```
现在 theme 变动就不会重新渲染整个组件树，因为 props.children 由外部传递进来，并没有发生变动。

其实上面的代码还有另外一个比较难发现的陷阱(官方文档也有提到):
```javascript
export function ThemeProvider(props) {
  const [theme, switchTheme] = useState(redTheme);
  return (
    {/*    这里每一次渲染ThemeProvider, 都会创建一个新的value(即使theme和switchTheme没有变动),
        从而导致强制渲染所有依赖该Context的组件 */}
    <Context.Provider value={{ theme, switchTheme }}>
      {props.children}
    </Context.Provider>
  );
}
```
#### 所以传递给 Context 的 value 最好做一下缓存:
```javascript
export function ThemeProvider(props) {
  const [theme, switchTheme] = useState(redTheme);
  const value = useMemo(() => ({ theme, switchTheme }), [theme]);
  return <Context.Provider value={value}>{props.children}</Context.Provider>;
}
```
原文地址:https://zhuanlan.zhihu.com/p/74229420


#### 16.⽂件上传如何做断点续传

#### 前端
前端大文件上传网上的大部分文章已经给出了解决方案，核心是利用 `Blob.prototype.slice` 方法，和数组的 `slice` 方法相似，调用的 `slice` 方法可以返回原文件的某个切片

这样我们就可以根据预先设置好的切片最大数量将文件切分为一个个切片，然后借助 `http` 的可并发性，同时上传多个切片，这样从原本传一个大文件，变成了同时传多个小的文件切片，可以大大减少上传时间

另外由于是并发，传输到服务端的顺序可能会发生变化，所以我们还需要给每个切片记录顺序

#### 服务端

服务端需要负责接受这些切片，并在接收到所有切片后合并切片

这里又引伸出两个问题

何时合并切片，即切片什么时候传输完成
如何合并切片

第一个问题需要前端进行配合，前端在每个切片中都携带切片最大数量的信息，当服务端接受到这个数量的切片时自动合并，也可以额外发一个请求主动通知服务端进行切片的合并

第二个问题，具体如何合并切片呢？这里可以使用 `nodejs` 的 `api fs.appendFileSync`，它可以同步地将数据追加到指定文件，也就是说，当服务端接受到所有切片后，先创建一个最终的文件，然后将所有切片逐步合并到这个文件中



#### 17.通过什什么做到并发请求?

使用异步`Prmosie All`或者`web worker`

#### 18.`redux`的主要作用和使用方式?

主要作用是：把所有的`state`集中到组件顶部，能够灵活的将所有`state`各取所需的分发给所有的组件。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/954a48b43b834aabb09656f11d574282~tplv-k3u1fbpfcp-zoom-1.image)

`store`: 保存数据的地方。整个应用智能有一个`Store`。
`state`： 包含所有数据，一个`state`对应一个view。只要`state`相同，`view`就相同。
`action`：`View`发出的通知`action`，改变state，从而改变`view`。修改state的唯一办法是使用`Action`。

#### 19.`React`组件通信如何实现？

### React组件间通信⽅式:

#### 1. ⽗组件向⼦组件通讯: 

⽗组件可以向⼦组件通过传props的⽅式，向⼦组件进⾏通讯

#### 2. ⼦组件向⽗组件通讯: 

`props`+回调的⽅式,⽗组件向⼦组件传递`props`进⾏通讯，此`props`为作⽤域为⽗组件⾃身的函数，⼦组件调⽤该函数，将⼦组件想要传递的信息，作为参数，传递到⽗组件的作⽤域中

#### 3. 兄弟组件通信: 

找到这两个兄弟节点共同的⽗节点,结合上⾯两种⽅式由⽗节点转发信息进⾏通信

#### 4. 跨层级通信:

`Context`设计⽬的是为了共享那些对于⼀个组件树⽽⾔是“全局”的数据，例如当前认证的⽤户、主题或⾸选语⾔,对于跨越多层的全局数据通过 `Context`通信再适合不过

如果你使用 16.3 以上版本的话，对于这种情况可以使用 Context API。
```javascript
// 创建 Context，可以在开始就传入值
const StateContext = React.createContext()
class Parent extends React.Component {
  render () {
    return (
      // value 就是传入 Context 中的值
      <StateContext.Provider value='yck'>
        <Child />
      </StateContext.Provider>
    )
  }
}
class Child extends React.Component {
  render () {
    return (
      <ThemeContext.Consumer>
        // 取出值
        {context => (
          name is { context }
        )}
      </ThemeContext.Consumer>
    );
  }
}
```
#### 5.发布订阅模式: 

发布者发布事件，订阅者监听事件并做出反应,我们可以通过引⼊event模块进⾏通信

#### 6.全局状态管理⼯具: 

借助Redux或者Mobx等全局状态管理⼯具进⾏通信,这种⼯具会维护⼀个全局状态中⼼Store,并根据不同的事件产⽣新的状态



#### 20.`redux`中如何进⾏异步操作?

当然,我们可以在 `componentDidmount` 中直接进⾏请求⽆须借助`redux`.

但是在⼀定规模的项⽬中,上述⽅法很难进⾏异步流的管理,通常情况下我们会借助redux的异步中间件进⾏异步处理.

`redux`异步流中间件其实有很多,但是当下主流的异步中间件只有两种`redux-thunk`、`redux-saga`，当然`redux-observable`可能也有资格占据⼀席之地,其余的异步中间件不管是社区活跃度还是npm下载量都⽐较差了.


#### 21.`React`如何进⾏组件/逻辑复⽤?

抛开已经被官⽅弃⽤的Mixin,组件抽象的技术⽬前有三种⽐较主流:

1.⾼阶组件:
  * 属性代理
  * 反向继承
2.渲染属性(Render Props)
3.Hooks

##### 1.⾼阶组件

高阶组件（`HOC`）是 `React` 中用于复用组件逻辑的一种高级技巧。`HOC` 自身不是 `React API` 的一部分，它是一种基于 `React` 的组合特性而形成的设计模式。

简而言之，高阶组件就是一个函数，它接受一个组件为参数，返回一个新组件。


#### 高阶组件的定义形如下面这样:
```javascript
//接受一个组件 WrappedComponent 作为参数，返回一个新组件 Proxy
function HocComponent(WrappedComponent) {
    return class ProxyComponent extends React.Component {
        render() {
            return <WrappedComponent {...this.props}>
        }
    }
}
```
#### HOC的约定和注意事项

#### 约定

* 将不相关的 props 传递给被包裹的组件(HOC应透传与自身无关的 props)

* 最大化可组合性

* 包装显示名称以便轻松调试

#### 注意事项

* 不要在函数内修改原组件
* 使用反向继承方式时，会丢失原本的显示名
* 不要在render函数中使用HOC

#### 高阶组件的缺点：
* 难以溯源。如果原始组件`A`通过好几个`HOC`的构造，最终生成了组件`B`，不知道哪个属性来自于哪个`HOC`，需要翻看每个`HOC`才知道各自做了什么事情，使用了什么属性。
* `props`属性名的冲突。某个属性可能被多个`HOC`重复使用。
* 静态构建。新的组件是在页面构建之前生成，先有组件，后生成页面。




##### 2.渲染属性(Render Props)
#### 作用

* 功能的复用，与HOC类似。
* 组件间数据的单向传递。

#### 什么是Render Props？

是一个用于告知组件要渲染什么内容的函数属性。该函数返回一个组件，是渲染出来的内容。
```javascript
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <p>位置：x:{ mouse.x } y: { mouse.y }</p>
    );
  }
}
 
class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }
 
  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }
 
  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>
 
        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}
 
class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>移动鼠标!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

通过以上`Demo`可以看到，`Mouse`组件通过`render`属性（属性名也可以是别的名字），指定了渲染哪个子组件，并且子组件可以接收参数值，进而实现内部逻辑。

再进行分析，发现Mouse组件提供可变数据源，是一个基础数据的提供者，最关键的代码是：

`{this.props.render(this.state)}`

通过render属性，将数据传递给另外一个组件。至于这个数据拿来干什么，怎么去渲染，就不是它管的事情了。

因此，整个页面很多地方可能需要用到鼠标坐标数据，以上的例子就可以实现功能的复用。


#### 相对高阶组件的优点：
* 不用担心props的命名冲突的问题
* 可以溯源，子组件的props一定来自父组件。
* 是动态构建的，页面在渲染后，可以动态地决定渲染哪个组件。
* 所有能用HOC完成的事情，Render Props都可以做，且更加灵活。
* 除了功能复用，还可以用作两个组件的单向数据传递。

##### 3.Hooks(推荐)

本来我以为renderProps除了一点点小小的瑕疵，基本上已经很完美了，然而正所谓没有对比就没有伤害，Hooks出来之后，前面的两个看似强大的模式都成了纸老虎。其他不说，首先从代码量上，Hooks就已经完胜了：


##### Hooks版本的计数器

```javascript
import React,{useState} from 'react'
import {View, Text,Button} from 'react-native'
 
export default function HookCount() {
    const [count,addCount,minusCount] = countNumber(0);
    return (
        <View style={{backgroundColor:theme,flex:1,alignItems:'center',justifyContent:'center'}}>
            <Text>You clicked {count} times</Text>
            <Button onPress={addCount} title={'add'}/>
            <Button onPress={minusCount} title={'minus'}/>
            <Button onPress={changeTheme} title={'ChangeTheme'}/>
        </View>
    );
}
 
function countNumber(initNumber) {
    const [count, setCount] = useState(initNumber);
    const addCount=()=> setCount(count + 1);
    const minusCount=()=>setCount(count -1);
    return [
        count,
        addCount,
        minusCount
    ]
}
```

在一个函数中定义的`state`，竟然可以直接拿到另一个函数中使用，然而有了`Hooks`，这种看似不可能的语法确实行得通。`Hooks`的优势不仅体现在代码量上，从风格上来说，也显得语义更清晰、结构更优雅（相比之下，高阶组件和`renderProps`的语法都显得比较诡异）。更重要的是，上述两种模式所拥有的种种缺点，它一个都没有。

当我们复用的逻辑达到多个的时候，这种优势会表现得更加明显。


#### 22.类组件和函数组件之间的区别是啥？

类组件可以使用其他特性，如状态 `state` 和生命周期钩子。

当组件只是接收 `props` 渲染到页面时，就是无状态组件，就属于函数组件，也被称为哑组件或展示组件。

函数组件和类组件当然是有区别的，而且函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。为了提高性能，尽量使用函数组件。

区别函数组件类组件是否有 `this`没有有是否有生命周期没有有是否有状态 `state`没有有


#### 23.你了解 `Virtual DOM` 吗？解释一下它的工作原理?

`Virtual DOM` 是一个轻量级的 JavaScript 对象，它最初只是 `real DOM` 的副本。它是一个节点树，它将元素、它们的属性和内容作为对象及其属性。 `React` 的渲染函数从 `React` 组件中创建一个节点树。然后它响应数据模型中的变化来更新该树，该变化是由用户或系统完成的各种动作引起的。

`Virtual DOM` 工作过程有三个简单的步骤。

1.每当底层数据发生改变时，整个 `UI` 都将在 `Virtual DOM` 描述中重新渲染。

![](https://img-blog.csdnimg.cn/20190325160134708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V5ZW9mYW5nZWw=,size_16,color_FFFFFF,t_70)

2.然后计算之前 DOM 表示与新表示的之间的差异。

![](https://img-blog.csdnimg.cn/20190325160145800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V5ZW9mYW5nZWw=,size_16,color_FFFFFF,t_70)

3.完成计算后，将只用实际更改的内容更新 real DOM。

![](https://img-blog.csdnimg.cn/20190325160158808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V5ZW9mYW5nZWw=,size_16,color_FFFFFF,t_70)

#### 24.`MVC`框架的主要问题是什么？

以下是`MVC`框架的一些主要问题：

* 对 `DOM` 操作的代价非常高
* 程序运行缓慢且效率低下
* 内存浪费严重
* 由于循环依赖性，组件模型需要围绕 `models` 和 `views` 进行创建

#### 25.解释 `Reducer` 的作用?

`Reducers` 是纯函数，它规定应用程序的状态怎样因响应 `ACTION` 而改变。`Reducers` 通过接受先前的状态和 `action` 来工作，然后它返回一个新的状态。它根据操作的类型确定需要执行哪种更新，然后返回新的值。如果不需要完成任务，它会返回原来的状态。


#### 26.`Redux` 有哪些优点？

`Redux` 的优点如下：

* 结果的可预测性 - 由于总是存在一个真实来源，即 `store` ，因此不存在如何将当前状态与动作和应用的其他部分同步的问题。

* 可维护性 - 代码变得更容易维护，具有可预测的结果和严格的结构。

* 服务器端渲染 - 你只需将服务器上创建的 `store` 传到客户端即可。这对初始渲染非常有用，并且可以优化应用性能，从而提供更好的用户体验。

* 开发人员工具 - 从操作到状态更改，开发人员可以实时跟踪应用中发生的所有事情。

* 社区和生态系统 - `Redux` 背后有一个巨大的社区，这使得它更加迷人。一个由才华横溢的人组成的大型社区为库的改进做出了贡献，并开发了各种应用。

* 易于测试 - `Redux` 的代码主要是小巧、纯粹和独立的功能。这使代码可测试且独立。
组织 - `Redux` 准确地说明了代码的组织方式，这使得代码在团队使用时更加一致和简单。

#### 27.为什么`React` `Router` 中使用 `switch` 关键字 ？

虽然 `<div> **` 用于封装 `Router` 中的多个路由，当你想要仅显示要在多个定义的路线中呈现的单个路线时，可以使用 `“switch”` 关键字。使用时，`<switch>**` 标记会按顺序将已定义的 `URL` 与已定义的路由进行匹配。找到第一个匹配项后，它将渲染指定的路径。从而绕过其它路线。

#### 28.说说`React Router` 的优点？

#### 几个优点是：

1.就像 `React` 基于组件一样，在 `React Router` `v4` 中，API 是 `‘All About Components’`。可以将 `Router` 可视化为单个根组件（<BrowserRouter>），其中我们将特定的子路由（<route>）包起来。

2.无需手动设置历史值：在 `React Router` `v4` 中，我们要做的就是将路由包装在 `<BrowserRouter>` 组件中。

3.包是分开的：共有三个包，分别用于 `Web`、`Native` 和 `Core`。这使我们应用更加紧凑。基于类似的编码风格很容易进行切换。


#### 29.列出 `Redux` 的组件!

#### `Redux` 由以下组件组成：

* `Action` – 这是一个用来描述发生了什么事情的对象。
* `Reducer` – 这是一个确定状态将如何变化的地方。
* `Store` – 整个程序的状态/对象树保存在`Store`中。
* `View` – 只显示 Store 提供的数据。

#### 30.(在构造函数中)调用 `super(props)` 的目的是什么?

1.在 super()被调用之前，子类是不能使用 this 的，在 ES2015 中，子类必须在 constructor中调用 super()。

2.传递props 给 super()的原因则是便于(在子类中)能在 constructor 访问 this.props。


#### 31.除了在构造函数中绑定 `this`，还有其它方式吗?

你可以使用属性初始值设定项(`property initializers`)来正确绑定回调，`create-react-app` 也是默认支持的。

在回调中你可以使用箭头函数，但问题是每次组件渲染时都会创建一个新的回调。


#### 32.如何告诉`react`它应该是编译生产环境版本？

通常情况下我们会使用 `Webpack` 的 `DefinePlugin` 方法来将 `NODE_ENV` 变量值设置为 `production`。

编译版本中 `React` 会忽略 `propType`验证以及其他的告警信息，同时还会降低代码库的大小，`React` 使用了 `Uglify` 插件来移除生产环境下不必要的注释等信息。

>NODE_ENV是怎么来的?

将生产和开发环境分开无疑是一个不错的实践(比如你是用TS写的源码, 在实际应用到生产时已经被编译成了`JS`, 源文件毫无用处, 不需要包含进去, 所以生产环境就排除掉`TS`文件以压缩体积), 落实到`node`具体实现也有很多方法

`NODE_ENV`最早是`express(web框架)`自己定的一个环境变量, 通过设置不同的值以在生产和开发环境作出相应动作. 随着该框架的流行, 通过该值的设置区分生产和开发环境变得广为接受, 很多工具也遵循了该做法


#### 33.`shouldComponentUpdate` 的作用?
`shouldComponentUpdate` 允许我们手动地判断是否要进行组件更新根据组件的应用场景设置函数的合理返回值能够帮我们避免不必要的更新


#### 34.`createElement` 与 `cloneElement` 的区别是什么?
`createElement` 函数是 JSX 编译之后使用的创建 `React Element `的函数。

`cloneElement` 则是用于复制某个元素并传入新的 `Props`。

#### 35.你用过哪些`redux`中间件?

中间件提供第三方插件的模式自定义拦截 
```
action -> reducer 
```

的过程。
#### 变为 
```
action -> middlewares -> reducer。
```

这种机制可以让我们改变数据流实现如异步action action 过滤日志输出异常报告等功能

1.redux-logger提供日志输出

2.redux-thunk处理异步操作

3.redux-promise处理异步操作actionCreator的返回值是promise

#### 36.`redux`有什么缺点?
一个组件所需要的数据必须由父组件传过来而不能像`flux`中直接从`store`取。

当一个组件相关数据更新时即使父组件不需要用到这个组件父组件还是会重新`render`可能会有效率影响或者需要写复杂的`shouldComponentUpdate`进行判断。


#### 37.`vue`和`react`的区别?


##### 1.监听数据变化的实现原理不同
Vue通过 getter/setter以及一些函数的劫持，能精确知道数据变化。

React默认是通过比较引用的方式（diff）进行的，如果不优化可能导致大量不必要的VDOM的重新渲染。为什么React不精确监听数据变化呢？这是因为Vue和React设计理念上的区别，Vue使用的是可变数据，而React更强调数据的不可变，两者没有好坏之分，Vue更加简单，而React构建大型应用的时候更加鲁棒。

##### 2.数据流的不同

![](https://img-blog.csdnimg.cn/20190626150140570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MTkwMTc3,size_16,color_FFFFFF,t_70 "")

Vue1.0中可以实现两种双向绑定：父子组件之间，props可以双向绑定；组件与DOM之间可以通过v-model双向绑定。Vue2.x中去掉了第一种，也就是父子组件之间不能双向绑定了（但是提供了一个语法糖自动帮你通过事件的方式修改），并且Vue2.x已经不鼓励组件对自己的 props进行任何修改了。

React一直不支持双向绑定，提倡的是单向数据流，称之为onChange/setState()模式。不过由于我们一般都会用Vuex以及Redux等单向数据流的状态管理框架，因此很多时候我们感受不到这一点的区别了。

##### 3.HoC和mixins

Vue组合不同功能的方式是通过mixins，Vue中组件是一个被包装的函数，并不简单的就是我们定义组件的时候传入的对象或者函数。比如我们定义的模板怎么被编译的？比如声明的props怎么接收到的？这些都是vue创建组件实例的时候隐式干的事。由于vue默默帮我们做了这么多事，所以我们自己如果直接把组件的声明包装一下，返回一个HoC，那么这个被包装的组件就无法正常工作了。

React组合不同功能的方式是通过HoC(高阶组件）。React最早也是使用mixins的，不过后来他们觉得这种方式对组件侵入太强会导致很多问题，就弃用了mixins转而使用HoC。高阶组件本质就是高阶函数，React的组件是一个纯粹的函数，所以高阶函数对React来说非常简单。

##### 4.组件通信的区别

![](https://img-blog.csdnimg.cn/20190626150244778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MTkwMTc3,size_16,color_FFFFFF,t_70 "")

Vue中有三种方式可以实现组件通信：父组件通过props向子组件传递数据或者回调，虽然可以传递回调，但是我们一般只传数据；子组件通过事件向父组件发送消息；通过V2.2.0中新增的provide/inject来实现父组件向子组件注入数据，可以跨越多个层级。

React中也有对应的三种方式：父组件通过props可以向子组件传递数据或者回调；可以通过 context 进行跨层级的通信，这其实和 provide/inject 起到的作用差不多。React 本身并不支持自定义事件，而Vue中子组件向父组件传递消息有两种方式：事件和回调函数，但Vue更倾向于使用事件。在React中我们都是使用回调函数的，这可能是他们二者最大的区别。

##### 5.模板渲染方式的不同

在深层上，模板的原理不同，这才是他们的本质区别：React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的，更加纯粹更加原生。而Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现对这一点，这样的做法显得有些独特，会把HTML弄得很乱。

举个例子，说明React的好处：react中render函数是支持闭包特性的，所以我们import的组件在render中可以直接调用。但是在Vue中，由于模板中使用的数据都必须挂在 this 上进行一次中转，所以我们import 一个组件完了之后，还需要在 components 中再声明下，这样显然是很奇怪但又不得不这样的做法。

##### 6.渲染过程不同

Vue可以更快地计算出Virtual DOM的差异，这是由于它在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。

React在应用的状态被改变时，全部子组件都会重新渲染。通过shouldComponentUpdate这个生命周期方法可以进行控制，但Vue将此视为默认的优化。

如果应用中交互复杂，需要处理大量的UI变化，那么使用Virtual DOM是一个好主意。如果更新元素并不频繁，那么Virtual DOM并不一定适用，性能很可能还不如直接操控DOM。

##### 7.框架本质不同

Vue本质是MVVM框架，由MVC发展而来；

React是前端组件化框架，由后端组件化发展而来。

##### 8.Vuex和Redux的区别

从表面上来说，store注入和使用方式有一些区别。

在Vuex中，`$store`被直接注入到了组件实例中，因此可以比较灵活的使用：使用dispatch、commit提交更新，通过mapState或者直接通过this.$store来读取数据。在Redux中，我们每一个组件都需要显示的用connect把需要的props和dispatch连接起来。

另外，Vuex更加灵活一些，组件中既可以dispatch action，也可以commit updates，而Redux中只能进行dispatch，不能直接调用reducer进行修改。

从实现原理上来说，最大的区别是两点：Redux使用的是不可变数据，而Vuex的数据是可变的，因此，Redux每次都是用新state替换旧state，而Vuex是直接修改。Redux在检测数据变化的时候，是通过diff的方式比较差异的，而Vuex其实和Vue的原理一样，是通过getter/setter来比较的，这两点的区别，也是因为React和Vue的设计理念不同。React更偏向于构建稳定大型的应用，非常的科班化。相比之下，Vue更偏向于简单迅速的解决问题，更灵活，不那么严格遵循条条框框。因此也会给人一种大型项目用React，小型项目用Vue的感觉。


原文:jianshu.com/p/eb06903c8bf7


#### 38、`React Portal` 有哪些使用场景

在以前 `react` 中所有的组件都会位于 `#app` 下而使用 `Portals` 提供了一种脱离 `#app` 的组件
因此 `Portals` 适合脱离文档流(`out of flow`) 的组件特别是 `position: absolute` 与 `position: fixed`的组件。比如模态框通知警告`goTop` 等。

以下是官方一个模态框的示例可以在以下地址中测试效果
```html
<html>
  <body>
    <div id="app"></div>
    <div id="modal"></div>
    <div id="gotop"></div>
    <div id="alert"></div>
  </body>
</html>
```

```javascript
const modalRoot = document.getElementById('modal');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el,
    );
  }
}
```
`React Hooks`当中的`useEffect`是如何区分生命周期钩子的

`useEffect`可以看成是`componentDidMountcomponentDidUpdate`和`componentWillUnmount`三者的结合。`useEffect(callback, [source])`接收两个参数调用方式如下
```javascript
 useEffect(() => {
   console.log('mounted');
   
   return () => {
       console.log('willUnmount');
   }
 }, [source]);
 ```

生命周期函数的调用主要是通过第二个参数[source]来进行控制有如下几种情况

* [source]参数不传时则每次都会优先调用上次保存的函数中返回的那个函数然后再调用外部那个函数
* [source]参数传[]时则外部的函数只会在初始化时调用一次返回的那个函数也只会最终在组件卸载时调用一次
* [source]参数有值时则只会监听到数组中的值发生变化后才优先调用返回的那个函数再调用外部的函数。


#### 39、什么是高阶组件(`HOC`)？
>高阶组件(Higher Order Componennt)本身其实不是组件而是一个函数这个函数接收一个元组件作为参数然后返回一个新的增强组件高阶组件的出现本身也是为了逻辑复用
### 举个例子
```javascript
function withLoginAuth(WrappedComponent) {
  return class extends React.Component {
      
      constructor(props) {
          super(props);
          this.state = {
            isLogin: false
          };
      }
      
      async componentDidMount() {
          const isLogin = await getLoginStatus();
          this.setState({ isLogin });
      }
      
      render() {
        if (this.state.isLogin) {
            return <WrappedComponent {...this.props} />;
        }
        
        return (<div>您还未登录...</div>);
      }
  }
}
```

#### 40、`React`实现的移动应用中如果出现卡顿有哪些可以考虑的优化方案？

* 增加`shouldComponentUpdate`钩子对新旧`props`进行比较如果值相同则阻止更新避免不必要的渲染或者使用`PureReactComponent`替代`Component`其内部已经封装了`shouldComponentUpdate`的浅比较逻辑

* 对于列表或其他结构相同的节点为其中的每一项增加唯一`key`属性以方便`React`的`diff`算法中对该节点的复用减少节点的创建和删除操作

render函数中减少类似
```JAVASCRIPT
onClick={() => {
    doSomething()
}}
```
的写法每次调用`render`函数时均会创建一个新的函数即使内容没有发生任何变化也会导致节点没必要的重渲染建议将函数保存在组件的成员对象中这样只会创建一次

* 组件的props如果需要经过一系列运算后才能拿到最终结果则可以考虑使用`reselect`库对结果进行缓存如果props值未发生变化则结果直接从缓存中拿避免高昂的运算代价

* webpack-bundle-analyzer分析当前页面的依赖包是否存在不合理性如果存在找到优化点并进行优化



#### 41. `React hooks`它带来了那些便利？
* 代码逻辑聚合逻辑复用
* `HOC`嵌套地狱
* 代替`class`
* `React` 中通常使用 类定义 或者 函数定义 创建组件:

在类定义中我们可以使用到许多 `React` 特性例如 `state`、 各种组件生命周期钩子等但是在函数定义中我们却无能为力因此 `React 16.8` 版本推出了一个新功能 (`React Hooks`)通过它可以更好的在函数定义组件中使用 `React` 特性。

### 好处:

* 跨组件复用: 其实 `render` `props` / `HOC` 也是为了复用相比于它们Hooks 作为官方的底层 `API`最为轻量而且改造成本小不会影响原来的* 组件层次结构和传说中的嵌套地狱
* 类定义更为复杂
* 不同的生命周期会使逻辑变得分散且混乱不易维护和管理
* 时刻需要关注`this`的指向问题
* 代码复用代价高高阶组件的使用经常会使整个组件树变得臃肿
* 状态与UI隔离: 正是由于 `Hooks` 的特性状态逻辑会变成更小的粒度并且极容易被抽象成一个自定义 `Hooks`组件中的状态和 `UI` 变得更为清晰和隔离。

### 注意:

避免在 循环/条件判断/嵌套函数 中调用 `hooks`保证调用顺序的稳定
只有 函数定义组件 和 `hooks` 可以调用 `hooks`避免在 类组件 或者 普通函数 中调用
不能在`useEffect`中使用`useStateReact` 会报错提示
类组件不会被替换或废弃不需要强制改造类组件两种方式能并存
重要钩子

状态钩子 (`useState`): 用于定义组件的`State`其到类定义中`this.state`的功能

```javascript
// useState 只接受一个参数: 初始状态
// 返回的是组件名和更改该组件对应的函数
const [flag, setFlag] = useState(true);
// 修改状态
setFlag(false)
	
// 上面的代码映射到类定义中:
this.state = {
	flag: true	
}
const flag = this.state.flag
const setFlag = (bool) => {
    this.setState({
        flag: bool,
    })
}
```

#### 生命周期钩子 (useEffect):

 类定义中有许多生命周期函数而在 `React Hooks` 中也提供了一个相应的函数 (`useEffect`)这里可以看做`componentDidMount`、`componentDidUpdate`和`componentWillUnmount`的结合。

* `useEffect(callback, [source])`接受两个参数
* `callback`: 钩子回调函数
* `source`: 设置触发条件仅当 `source` 发生改变时才会触发
* `useEffect`钩子在没有传入[source]参数时默认在每次 `render` 时都会优先调用上次保存的回调中返回的函数后再重新调用回调
```javascript
useEffect(() => {
	// 组件挂载后执行事件绑定
	console.log('on')
	addEventListener()
	// 组件 update 时会执行事件解绑
	return () => {
		console.log('off')
		removeEventListener()
	}
}, [source]);


// 每次 source 发生改变时执行结果(以类定义的生命周期便于大家理解):
// --- DidMount ---
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- WillUnmount --- 
// 'off'

```

通过第二个参数我们便可模拟出几个常用的生命周期:

* `componentDidMount`: 传入[]时就只会在初始化时调用一次
* `const useMount = (fn) => useEffect(fn, [])`
* `componentWillUnmount`: 传入[]回调中的返回的函数也只会被最终执行一次
* `const useUnmount = (fn) => useEffect(() => fn, [])`
* `mounted`: 可以使用 useState 封装成一个高度可复用的 `mounted` 状态
```javascript
const useMounted = () => {
    const [mounted, setMounted] = useState(false);
    useEffect(() => {
        !mounted && setMounted(true);
        return () => setMounted(false);
    }, []);
    return mounted;
}
componentDidUpdate: useEffect每次均会执行其实就是排除了 DidMount 后即可
const mounted = useMounted() 
useEffect(() => {
    mounted && fn()
})
```

#### 其它内置钩子:
* `useContext`: 获取 `context` 对象

* `useReducer`: 类似于 `Redux` 思想的实现但其并不足以替代 Redux可以理解成一个组件内部的 `redux`:
并不是持久化存储会随着组件被销毁而销毁

* 属于组件内部各个组件是相互隔离的单纯用它并无法共享数据

* 配合`useContext`的全局性可以完成一个轻量级的 `Redux`(`easy-peasy`)
* `useCallback`: 缓存回调函数避免传入的回调每次都是新的函数实例而导致依赖组件重新渲染具有性能优化的效果
* `useMemo`: 用于缓存传入的 `props`避免依赖的组件每次都重新渲染
* `useRef`: 获取组件的真实节点
* `useLayoutEffect`
* DOM更新同步钩子。用法与useEffect类似只是区别于执行时间点的不同
* useEffect属于异步执行并不会等待 DOM 真正渲染后执行而`useLayoutEffect`则会真正渲染后才触发
* 可以获取更新后的 `state`
* 自定义钩子(`useXxxxx`): 基于 `Hooks` 可以引用其它 `Hooks` 这个特性我们可以编写自定义钩子如上面的`useMounted`。又例如我们需要每个页面自定义标题:

```javascript
function useTitle(title) {
  useEffect(
    () => {
      document.title = title;
    });
}

// 使用:
function Home() {
	const title = '我是首页'
	useTitle(title)
	
	return (
		<div>{title}</div>
	)
}
```

#### 42、`pureComponent`和`FunctionComponent`区别？

`PureComponent`和`Component`完全相同

但是在`shouldComponentUpdate`实现中`PureComponent`使用了`props`和`state`的浅比较。

主要作用是用来提高某些特定场景的性能


#### 43、`Redux`实现原理？

#### 为什么要用redux

在`React`中数据在组件中是单向流动的数据从一个方向父组件流向子组件通过`props`,所以两个非父子组件之间通信就相对麻烦`redux`的出现就是为了解决`state`里面的数据问题

#### Redux设计理念

`Redux`是将整个应用状态存储到一个地方上称为`store`,里面保存着一个状态树`store` `tree`,组件可以派发(`dispatch`)行为(`action`)给`store`,而不是直接通知其他组件组件内部通过订阅`store`中的状态`state`来刷新自己的视图

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2008181824_1597746275324.jpg)

image

#### Redux三大原则

#### 1.唯一数据源

整个应用的`state`都被存储到一个状态树里面并且这个状态树只存在于唯一的`store`中

#### 2.保持只读状态

`state`是只读的唯一改变`state`的方法就是触发`actionaction`是一个用于描述以发生时间的普通对象

#### 3.数据改变只能通过纯函数来执行

使用纯函数来执行修改为了描述`action`如何改变`state`的你需要编写`reducers`

Redux源码
```javascript
let createStore = (reducer) => {
    let state;
    //获取状态对象
    //存放所有的监听函数
    let listeners = [];
    let getState = () => state;
    //提供一个方法供外部调用派发action
    let dispath = (action) => {
        //调用管理员reducer得到新的state
        state = reducer(state, action);
        //执行所有的监听函数
        listeners.forEach((l) => l())
    }
    //订阅状态变化事件当状态改变发生之后执行监听函数
    let subscribe = (listener) => {
        listeners.push(listener);
    }
    dispath();
    return {
        getState,
        dispath,
        subscribe
    }
}
let combineReducers=(renducers)=>{
    //传入一个renducers管理组返回的是一个renducer
    return function(state={},action={}){
        let newState={};
        for(var attr in renducers){
            newState[attr]=renducers[attr](state[attr],action)

        }
        return newState;
    }
}
export {createStore,combineReducers};
```


#### 44、`connect`原理

首先`connect`之所以会成功是因为`Provider`组件
在原应用组件上包裹一层使原来整个应用成为`Provider`的子组件
接收`Redux`的`store`作为`props`通过`context`对象传递给子孙组件上的`connect`
connect做了些什么。它真正连接 `Redux `和 `React`它包在我们的容器组件的外一层它接收上面 `Provider` 提供的 `store` 里面的`state` 和 `dispatch`传给一个构造函数返回一个对象以属性形式传给我们的容器组件

`connect`是一个高阶函数首先传入mapStateToProps、mapDispatchToProps然后返回一个生产Component的函数(wrapWithConnect)然后再将真正的Component作为参数传入wrapWithConnect这样就生产出一个经过包裹的Connect组件
该组件具有如下特点

通过`props.store`获取祖先`Component`的`store props`包括`stateProps`、`dispatchProps`、`parentProps`,合并在一起得到`nextState`作为`props`传给真正的`Component componentDidMount`时添加事件`this.store.subscribe(this.handleChange)`实现页面交互
`shouldComponentUpdate`时判断是否有避免进行渲染提升页面性能并得到nextState componentWillUnmount时移除注册的事件`this.handleChange`

由于connect的源码过长我们只看主要逻辑
```javascript
export default function connect(mapStateToProps, mapDispatchToProps, mergeProps, options = {}) {
  return function wrapWithConnect(WrappedComponent) {
    class Connect extends Component {
      constructor(props, context) {
        // 从祖先Component处获得store
        this.store = props.store || context.store
        this.stateProps = computeStateProps(this.store, props)
        this.dispatchProps = computeDispatchProps(this.store, props)
        this.state = { storeState: null }
        // 对stateProps、dispatchProps、parentProps进行合并
        this.updateState()
      }
      shouldComponentUpdate(nextProps, nextState) {
        // 进行判断当数据发生改变时Component重新渲染
        if (propsChanged 
        || mapStateProducedChange 
        || dispatchPropsChanged) {
          this.updateState(nextProps)
            return true
          }
        }
        componentDidMount() {
          // 改变Component的state
          this.store.subscribe(() = {
            this.setState({
              storeState: this.store.getState()
            })
          })
        }
        render() {
          // 生成包裹组件Connect
          return (
            <WrappedComponent {...this.nextState} />
          )
        }
      }
      Connect.contextTypes = {
        store: storeShape
      }
      return Connect;
    }
  }
 ```

#### 45、`react` 的渲染过程中兄弟节点之间是怎么处理的也就是key值不一样的时候？

通常我们输出节点的时候都是`map`一个数组然后返回一个`ReactNode`为了方便`react`内部进行优化

我们必须给每一个`reactNode`添加`key`这个`key prop`在设计值处不是给开发者用的而是给react用的大概的作用就是给每一个reactNode添加一个身份标识

方便react进行识别在重渲染过程中如果key一样若组件属性有所变化则`react`只更新组件对应的属性没有变化则不更新如果`key`不一样则`react`先销毁该组件然后重新创建该组件

#### 46、`react-router`里的`<Link>`标签和`<a>`标签有什么区别？

`react-router`是伴随着`react`框架出现的路由系统，它也是公认的一种优秀的路由解决方案。

在使用`react-router`时候，我们常常会使用其自带的路径跳转组件`Link`,通过`<Link to="path"></Link>`实现跳转，对比a标签 ,`Link`组件避免了不必要的重渲染。


#### 47、`react` 的虚拟`dom`是怎么实现的？

首先说说为什么要使用`Virturl DOM`因为操作真实`DOM`的耗费的性能代价太高

所以`react`内部使用`js`实现了一套`dom`结构在每次操作在和真实`dom`之前使用实现好的`diff`算法对虚拟`dom`进行比较递归找出有变化的dom节点然后对其进行更新操作。

为了实现虚拟`DOM`我们需要把每一种节点类型抽象成对象每一种节点类型有自己的属性也就是`prop`每次进行`diff`的时候`react`会先比较该节点类型

假如节点类型不一样那么`react`会直接删除该节点然后直接创建新的节点插入到其中假如节点类型一样那么会比较`prop`是否有更新假如有`prop`不一样那么`react`会判定该节点有更新那么重渲染该节点然后在对其子节点进行比较一层一层往下直到没有子节点

#### 48、如何告诉 `React` 它应该编译生产环境版？
通常情况下我们会使用 `Webpack` 的 `DefinePlugin` 方法来将 `NODE_ENV` 变量值设置为 `production`。

编译版本中 `React`会忽略 `propType` 验证以及其他的告警信息同时还会降低代码库的大小React 使用了 `Uglify` 插件来移除生产环境下不必要的注释等信息



#### 49、`setState`到底是异步还是同步?

#### 有时表现出异步,有时表现出同步

1.`setState`只在合成事件和钩子函数中是“异步”的在原生事件和`setTimeout` 中都是同步的

2.`setState` 的“异步”并不是说内部由异步代码实现其实本身执行的过程和代码都是同步的

只是合成事件和钩子函数的调用顺序在更新之前导致在合成事件和钩子函数中没法立马拿到更新后的值

3.形成了所谓的`异步`当然可以通过第二个参数`setState(partialState, callback)`中的`callback`拿到更新后的结果

4.`setState` 的批量更新优化也是建立在“异步”合成事件、钩子函数之上的在原生事件和`setTimeout` 中不会批量更新在“异步”中如果对同一个值进行多次`setState`的批量更新策略会对其进行覆盖取最后一次的执行如果是同时`setState`多个不同的值在更新时会对其进行合并批量更新

#### 50、你对 `Time Slice`的理解?

#### 时间分片

* React 在渲染render的时候不会阻塞现在的线程
* 如果你的设备足够快你会感觉渲染是同步的
* 如果你设备非常慢你会感觉还算是灵敏的
* 虽然是异步渲染但是你将会看到完整的渲染而不是一个组件一行行的渲染出来
* 同样书写组件的方式
* 也就是说这是React背后在做的事情对于我们开发者来说是透明的具体是什么样的效果呢


#### 51、说说你用`react`有什么坑点？
1. JSX做表达式判断时候需要强转为boolean类型

如果不使用 !!b 进行强转数据类型会在页面里面输出 0。
```javascript
render() {
  const b = 0;
  return <div>
    {
      !!b && <div>这是一段文本</div>
    }
  </div>
}
```
2. 尽量不要在 `componentWillReviceProps` 里使用 `setState`如果一定要使用那么需要判断结束条件不然会出现无限重渲染导致页面崩溃

3. 给组件添加ref时候尽量不要使用匿名函数因为当组件更新的时候匿名函数会被当做新的`prop`处理让`ref`属性接受到新函数的时候`react`内部会先清空`ref`也就是会以`null`为回调参数先执行一次`ref`这个`props`然后在以该组件的实例执行一次`ref`所以用匿名函数做ref的时候有的时候去`ref`赋值后的属性会取到`null`

4. 遍历子节点的时候不要用 index 作为组件的 key 进行传入

#### 52、`react`性能优化方案？
* 重写`shouldComponentUpdate`来避免不必要的`dom`操作
* 使用` production` 版本的`react.js`
* 使用`key`来帮助`React`识别列表中所有子组件的最小变化

#### 53、`diff`算法?
* 把树形结构按照层级分解只比较同级元素。
* 给列表结构的每个单元添加唯一的key属性方便比较。
* `React` 只会匹配相同 `class` 的 `component`这里面的class指的是组件的名字
* 合并操作调用` component` 的 `setState` 方法的时候, React 将其标记为 - `dirty`.到每一个事件循环结束, `React` 检查所有标记 `dirty`的 `component`重新绘制.
* 选择性子树渲染。开发人员可以重写`shouldComponentUpdate`提高`diff`的性能

#### 54、传入 `setState` 函数的第二个参数的作用是什么？
该函数会在 setState 函数调用完成并且组件开始重渲染的时候被调用我们可以用该函数来监听渲染是否完成
```javascript
this.setState(
  { username: 'tylermcginnis33' },
  () => console.log('setState has finished and the component has re-rendered.')
)
this.setState((prevState, props) => {
  return {
    streak: prevState.streak + props.count
  }
})
```

#### 55、为什么虚拟`dom`会提高性能？
>虚拟dom相当于在js和真实dom中间加了一个缓存利用dom diff算法避免了没有必要的dom操作从而提高性能

#### 具体实现步骤如下

* 用 `JavaScript `对象结构表示 DOM 树的结构然后用这个树构建一个真正的 `DOM` 树插到文档当中
* 当状态变更的时候重新构造一棵新的对象树。然后用新的树和旧的树进行比较记录两棵树差异
* 把2所记录的差异应用到步骤1所构建的真正的`DOM`树上视图就更新

#### 56、我现在有一个`button`要用`react`在上面绑定点击事件要怎么做？
```javascript
class Demo {
  render() {
    return <button onClick={(e) => {
      alert('我点击了按钮')
    }}>
      按钮
    </button>
  }
}
```
你觉得你这样设置点击事件会有什么问题吗

由于`onClick`使用的是匿名函数所有每次重渲染的时候会把该`onClick`当做一个新的prop来处理会将内部缓存的`onClick`事件进行重新赋值所以相对直接使用函数来说可能有一点的性能下降

修改
```javascript
class Demo {

  onClick = (e) => {
    alert('我点击了按钮')
  }

  render() {
    return <button onClick={this.onClick}>
      按钮
    </button>
  }
  ```
  
#### 57、`react`性能优化是哪个周期函数？
`shouldComponentUpdate` 这个方法用来判断是否需要调用`render`方法重新描绘`dom`。因为dom的描绘非常消耗性能如果我们能在

`shouldComponentUpdate`方法中能够写出更优化的`dom diff`算法可以极大的提高性能

#### 58、概述下 `React` 中的事件处理逻辑？

为了解决跨浏览器兼容性问题`React` 会将浏览器原生事件`Browser Native Event`封装为合成事件`SyntheticEvent`传入设置的事件处理器中。

这里的合成事件提供了与原生事件相同的接口不过它们屏蔽了底层浏览器的细节差异保证了行为的一致性。另外有意思的是React 并没有直接将事件附着到子元素上而是以单一事件监听器的方式将所有的事件发送到顶层进行处理。

这样 `React` 在更新 `DOM` 的时候就不需要考虑如何去处理附着在 `DOM` 上的事件监听器最终达到优化性能的目的

* Flux 的最大特点就是数据的`单向流动`。
* 用户访问 `View`
* View发出用户的 `Action`
* `Dispatcher` 收到Action要求 `Store` 进行相应的更新
* `Store` 更新后发出一个"`change`"事件
* `View` 收到"`change`"事件后更新页面

#### 59、`React`如何进行组件/逻辑复用?
抛开已经被官方弃用的Mixin,组件抽象的技术目前有三种比较主流:

高阶组件:
* 属性代理
* 反向继承
* 渲染属性
* react-hooks

#### 60.什么是虚拟`DOM`？

虚拟`DOM`的内涵和外延？

#### 内涵
虚拟`DOM`它是真实`DOM`的内存表示,一种编程概念，一种模式。它会和真实的`DOM`同步，比如通过`ReactDOM`这种库，这个同步的过程叫做调和(`reconcilation`)。

描述`HTML`标签，使用`JS`对象来表示。

虚拟`DOM`更多是一种模式，不是一种特定的技术。

#### 外延
它的外延便是`javaScript`对象,而`React`返回的`React`元素也是对象，层层嵌套，就像`DOM`树。

#### 虚拟`DOM`提出，解决了什么问题？
一个技术提出，背后有它的故事的。是什么呢？怎么会提出虚拟`DOM`？当时技术解不了什么问题？
提出了虚拟DOM,那怎么去实现一套技术呢？怎么声明语法？

在`React`世界里，虚拟`DOM`跟`React`元素关联在一起的，因为它们都表达了用户界面的对象。
这个编程概念提出，解放了对`DOM`细节的操作，无须关心具体`DOM`的A`PI`,只需要描述，我想要什么样的页面，告诉React，它会帮我们去渲染。

#### 虚拟`DOM`之上延伸了什么概念？
由于是虚拟DOM，延伸了React的API是声明式的，我们不需要具体操作`DOM`。`ReactDOM`会操作`DOM`。

虚拟`DOM`是`JavaScript`对象的表示。



###  61. `React`声明周期及自己的理解

React生命周期

###  旧版

![](https://user-gold-cdn.xitu.io/2019/5/12/16aaa60b801c4411?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

初始化阶段(Initalization)

`setup props ande state`(设置组件的初始化属性和状态)

初始化状态对象
```javascript
static defaultProps = {
    name:'计数器'  //初始化默认的属性对象
}
```
###  初始化属性对象
```javascript
constructor(props){
    super(props);
    this.state = {
        number:0
    }
}
```
###  挂载阶段(Mounting)
挂载：将虚拟DOM转化为真实DOM的过程

###  componentWillMount
组件挂载之前，在渲染过程中可能会执行多次，不推荐使用

###  render
组件挂载

###  componentDidMount
组件挂载之后，永远只会执行一次，推荐在此阶段执行副作用，进行异步操作。比如发ajax请求，操作DOM等

更新阶段(Updation)</br>
属性更新(props变化)</br>

###  componentWillReceiveProps

组件收到新的属性对象时调用，首次渲染不会触发

###  shouldComponentUpdate
询问组件是否可以更新


参数：新的属性对象</br>
返回：boolean值</br>

true允许更新继续向下执行</br>
false不允许更新，停止执行，不会调用之后的生命周期函数</br>

###  componentWillUpdate

组件更新之前

render
根据新的属性对象重新挂载(渲染)组件</br>

componentDidUpdate</br>
组件更新完成</br>

状态更新(state变化)</br>
shouldComponentUpdate</br>

询问组件是否可以更新

参数：新的状态对象</br>
返回：boolean值</br>

true允许更新继续向下执行</br>
false不允许更新，停止执行，不会调用之后的生命周期函数</br>


###  componentWillUpdate

组件更新之前</br>
render</br>

根据新的状态对象重新挂载(渲染)组件

###  componentDilUpdtae
组件更新完成</br>

卸载阶段(Unmounting)

###  componentWillUnmount

组件卸载之前调用

放码过来

有兴趣的朋友可以根据以下代码进行测试
```javascript
import React, { Component } from 'react';
<!-- 父组件 -->
class Counter extends Component {
  static defaultProps = { //初始化默认的属性对象
    name:'计数器'
  }
  constructor(props) {
    super(props);
    this.state = { // 初始化默认的状态对象
      number:0
    }
    console.log('1. 父constructor初始化 props and state');
  }
  componentWillMount() {
    console.log('2. 父componentWillMount组件将要挂载');
  }
  componentDidMount() {
    console.log('4. 父componentDidMount组件挂载完成');
  }
  shouldComponentUpdate() {
    console.log('5. 父componentShouldUpdate询问组件是否可以更新');
    return true;
  }
  componentWillUpdate() {
    console.log('6. 父componentWillUpdate组件将要更新');
  }
  componentDidUpdate() {
    console.log('7. 父componentDidUpdate组件更新完成');
  }
  render() {
    console.log('3. 父render渲染，也就是挂载');
    const style = {display:'block'}
    return (
      <div style={{border:'10px solid green',pending:'10px'}}>
        {this.props.name}:{this.state.number}
        <button onClick={this.add} style={style}>+</button>
        {this.state.number %3 !== 0 && <SubCounter number={this.state.number}/>}
      </div>
    );
  }
  add = () => {
    this.setState({
      number:this.state.number+1
    })
  }
}
```
<!-- 子组件 -->
```javascript
class SubCounter extends Component {
  static defaultProps = {
    number:10
  }
  componentWillReceiveProps() {
    console.log('1. 子componentWillReceiveProps属性将要发生变化 ');
  }
  shouldComponentUpdate(nextProps, nextState) {
    console.log('2. 子componentShouldUpdate询问组件是否可以更新');
    // 调用此方法的时候会把新的属性对象和新的状态对象传递过来
    if (nextProps.number % 3 === 0) {
      return true;
    }
    return false;
  }
  componentWillUnmount() {
    console.log(' 3.子componentWillUnmount组件将要卸载 ');
  }
  render() {
    return (
      <div style={{border:'10px solid red'}}>
        {this.props.number}
      </div>
    );
  }
}
export default Counter;
```
###  代码新版

![](https://user-gold-cdn.xitu.io/2019/5/12/16aab2d849e66f64?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


###  创建时
###  constructor

初始化属性和状态

###  getDerivedStateFromProps

根据属性对象派生状态对象


###  静态方法
参数：新的属性对象，旧的状态对象

用途：在没有这个生命周期函数之前，我们使用的数据来源可能是属性对象，也可能是状态对象，在我们的上文中的放码过来阶段就有所体现，我们可以通过这个生命周期函数将属性对象派生到状态对象上，使我们在代码中只通过this.state.XXX来绑定我们的数据。

示例如下
```javascript
import React, { Component } from 'react';
/**
 * 父
 */
class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      number:0
    }
  }
  render() {
    return (
      <div>
        <p>{this.state.number}</p>
        <button onClick={this.add}>+</button>
        <SubCounter number={this.state.number}/>
      </div>
    );
  }
  add = () => {
    this.setState({
      number:this.state.number+1
    })
  }
}
/**
 * 子
 */
class SubCounter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      number:0
    }
  }
  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.number % 2 === 0) {
      return {number:nextProps.number*2}
    } else {
      return {number:nextProps.number*3}
    }
  }
  render() {
    console.log(this.state);
    return (
      <div>
        <p>{this.state.number}</p>
      </div>
    );
  }
}
export default Counter;
```
###  render
挂载(渲染)组件

componentDidMount</br>
组件挂载(渲染)完成</br>
更新时</br>

getDerivedStateFromProps</br>
根据属性对象派生状态对象</br>

静态方法</br>
参数：新的属性对象，旧的状态对象</br>

shouldComponentUpdate</br>
询问组件是否可以更新</br>


参数：新的状态对象</br>
返回：boolean值</br>

true允许更新继续向下执行</br>
false不允许更新，停止执行，不会调用之后的生命周期函数</br>


render</br>
根据新的状态对象重新挂载(渲染)组件</br>

getSnapshotbeforeUpdate</br>
获取更新前的快照</br>


举例：在我们的日常开发中你一定会遇到这个问题，异步加载资源，当浏览器处于非第一屏的时候并且所在屏之前的模块并没有加载出来，你肯定不希望在所在屏之前的模块加载出来之后浏览器显示窗口仍然处于当前的高度，而是希望仍然处于我们所浏览屏的高度，在没有这个生命周期之前react处理这个问题是很棘手的。


未使用getSnapshotbeforeUpdate
```javascript
import React, { Component } from 'react';
class GetSnapshotBeforeUpdate extends Component {
  constructor(props) {
    super(props);
    this.wrapper = React.createRef();
    this.state = {
      messages:['4','3','2','1','0']
    }
  }
  componentDidMount() {
    setInterval(() => {
      this.setState(() => {
        this.state.messages.unshift(this.state.messages.length);
      }, () => {
          this.setState({
            messages: this.state.messages
          })
      })
    },1000)
  }
  render() {
    let style = {
      height: '100px',
      width: '200px',
      border: '1px solid red',
      overflow:'auto'
    }
    return (
      <ul style={style} ref={this.wrapper}>
        {
          this.state.messages.map((message, index) => <li key={index}>{message}</li>)
        }
      </ul>
    );
  }
}
export default GetSnapshotBeforeUpdate;
```

使用getSnapshotbeforeUpdate
```javascript
import React, { Component } from 'react';
class GetSnapshotBeforeUpdate extends Component {
  getSnapshotBeforeUpdate() {
    // 返回更新内容的高度
    return this.wrapper.current.scrollHeight;
  }
  // 组件更新完毕
  componentDidUpdate(prevProps,prevState,prevScrollHeight) {
    console.log(prevProps,prevState,prevScrollHeight);
    this.wrapper.current.scrollTop = this.wrapper.current.scrollTop +
    (this.wrapper.current.scrollHeight - prevScrollHeight);
  }
export default GetSnapshotBeforeUpdate;
```
componentDidUpdate</br>
组件更新完成</br>

卸载时</br>
componentWillUnmount</br>

组件卸载之前

###  总结
数据获取为什么一定要在componentDidMount里面调用

###  constructor中

constructor()中获取数据的话，如果时间太长，或者出错，组件就渲染不出来，整个页面都没法渲染了

componentWillMount()

【1】如果使用SSR（服务端渲染）,componentWillMount会执行2次，一次在服务端，一次在客户端。而componentDidMount不会。

【2】React16之后采用了Fiber架构，只有componentDidMount声明周期函数是确定被执行一次的，类似componentWillMount的生命周期钩子都有可能执行多次，所以不加以在这些生命周期中做有副作用的操作，比如请求数据之类

componentDidMount()

【1】确保已经render过一次。提醒我们正确地设置初始状态，这样就不会得到导致错误的undefined状态。

【2】componentDidMount方法中的代码，是在组件已经完全挂载到网页上才会调用被执行，所以可以保证数据的加载。此外，在这方法中调用setState方法，会触发重渲染。所以，官方设计这个方法就是用来加载外部数据用的，或处理其他的副作用代码。

###  新旧比较(废三增二)

###  废弃了

componentWillMount</br>
componentWillReceiveProps</br>
componentWillUpdate</br>

###  增加了

getDerivedStateFromProps</br>
getSnapshotbeforeUpdate</br>



###  62.如何配置`React-Router`
###  路由的概念
路由的作用就是将url和函数进行映射，在单页面应用中路由是必不可少的部分，路由配置就是一组指令，用来告诉router如何匹配url，以及对应的函数映射，即执行对应的代码。

###  react-router
每一门JS框架都会有自己定制的router框架，react-router就是react开发应用御用的路由框架，目前它的最新的官方版本为4.1.2。本文给大家介绍的是react-router相比于其他router框架更灵活的配置方式，大家可以根据自己的项目需要选择合适的方式。

###  1.标签的方式
下面我们看一个例子：
```javascript
import { IndexRoute } from 'react-router'
const Dashboard = React.createClass({
 render() {
   return <div>Welcome to the app!</div>
 }
})
React.render((
 <Router>
   <Route path="/" component={App}>
     <IndexRoute component={Dashboard} />
     <Route path="about" component={About} />
     <Route path="inbox" component={Inbox}>
       <Route path="messages/:id" component={Message} />
     </Route>
   </Route>
 </Router>
), document.body)
```

我们可以看到这种路由配置方式使用Route标签，然后根据component找到对应的映射。

这里需要注意的是IndexRoute这个有点不一样的标签，这个的作用就是匹配’/’

的路径，因为在渲染App整个组件的时候，可能它的children还没渲染，就已经有’/'页面了，你可以把IndexRoute当成首页。

嵌套路由就直接在Route的标签中在加一个标签，就是这么简单

###  2.对象配置方式

有时候我们需要在路由跳转之前做一些操作，比如用户如果编辑了某个页面信息未保存，提醒它是否离开。react-router提供了两个hook，onLeave在所有将离开的路由触发，从最下层的子路由到最外层的父路由，onEnter在进入路由触发，从最外层的父路由到最下层的自路由。

让我们看代码：
```javascript
const routeConfig = [
 { path: '/',
   component: App,
   indexRoute: { component: Dashboard },
   childRoutes: [
     { path: 'about', component: About },
     { path: 'inbox',
       component: Inbox,
       childRoutes: [
         { path: '/messages/:id', component: Message },
         { path: 'messages/:id',
           onEnter: function (nextState, replaceState) {
             //do something
           }
         }
       ]
     }
   ]
 }
]
React.render(<Router routes={routeConfig} />, document.body)
```

###  3.按需加载的路由配置(重点了解，可以优化加载性能)
在大型应用中，性能是一个很重要的问题，按需要加载JS是一种优化性能的方式。

在React router中不仅组件是可以异步加载的，路由也是允许异步加载的。Route 可以定义 getChildRoutes，getIndexRoute 和 getComponents 这几个函数，他们都是异步执行的，并且只有在需要的时候才会调用。

我们看一个例子：
```javascript
const CourseRoute = {
 path: 'course/:courseId',
 getChildRoutes(location, callback) {
   require.ensure([], function (require) {
     callback(null, [
       require('./routes/Announcements'),
       require('./routes/Assignments'),
       require('./routes/Grades'),
     ])
   })
 },
 getIndexRoute(location, callback) {
   require.ensure([], function (require) {
     callback(null, require('./components/Index'))
   })
 },
 getComponents(location, callback) {
   require.ensure([], function (require) {
     callback(null, require('./components/Course'))
   })
 }
}
```

这种方式需要配合webpack中有实现代码拆分功能的工具来用，其实就是把路由拆分成小代码块，然后按需加载。

###  63. 路由的动态加载模块

使用suspense 和 lazy实现

###  renderRoutesMap.js
```javascript
import React, {lazy,Suspense} from 'React';
import {Route} from 'react-router-dom';

const RenderRoutesMap = (routes) => (
  const LazyComponent = lazy(route.component);
  routes.map((route,index) => {
    return(
      <Route key={index} exact = {route.exact} path={route.path} render={props =>(
        <Suspense>
          <LazyComponent  {...props}/>
        </Suspense>
      )}/>
    )
  })
)

export default RenderRoutesMap
```
###  renderRoutes.js
```javascript
import RenderRoutesMap from './renderRoutesMap'
import  {ConnectedRouter} from 'connected-react-router';
import {PropType} from 'prop-types'

RenderRoutes.propTypes = {
  routes: PropTypes.array.isRequired,
  extraProps: PropTypes.object.isRequired,
  switchProps: PropTypes.object.isRequired,
} 

const RenderRoutes = ({routes, extraProps = {}, switchProps = {} }) => (
  <ConnectedRouter {...extraProps}>
    <Switch {...switchProps}>
      {RenderRoutesMap(routes)}
    </Switch>
  </ConnectedRouter>
)

export default RenderRoutes
```

###  routes.js
```javascript
import RenderRoutes from './renderRoutes'
const router = (routerConfig, routerProps) => (
  renderRoutes({
    routes; routerConfig,
    extraProps:routerProps
  })
)
export default Routes
```
###  使用
// index.js
```javascript
import Routes from './routes'
import Home from './home'
import Test from './test'
const routers = [
  {
    path: '/home',
    exact: true,
    component: Home
  },
{
    path: '/test',
    exact: true,
    component: Test
  }
]
return(
  <Routes />
)
```



###  64.介绍`Redux`数据流的流程
####  1、数据流是什么？为什么要用数据流？</br>
1）数据流是行为与响应的抽象。</br>

用户在页面上输入表单、按下按钮、拖拽等行为，页面会根据用户的行为给出一些响应，如刷新、跳转、局部刷新、Ajax局部刷新、数据更新等。以对象、方法来把它们抽象出来，这就是数据流。

2）使用数据流可以帮助我们明确行为以及行为对应的响应。
这与React的目标——状态图预测是密不可分的。
 
####  2、React与数据流的关系
React是纯V的框架，只负责视图，把视图做成了组件化，不涉及任何的数据和控制，需要数据流进行支撑才能完成一个完整的前端项目。

####  3、主流的数据流框架有哪些？为什么使用Redux？
1）Flux：Facebook在推出React时推出了一套官方适配的、配合React来实现的数据流，现在是一种单项数据流或单向数据绑定的思想，但它非常重、非常大、实用性不太强，因此很多人做了很多改进，出现了很多第三方的数据流框架，比较出名的有reFlux和Redux。
2）reflux
3）Redux：在很多第三方数据流框架中有一些优势：一是非常简单；二是它维护了一棵单一的状态树。
4、单向数据流：

####  1）MVC
数据放在Model里，View来控制显示，Controller来做整体的管理。
随着系统的庞大，会产生一些问题：交互时，当用户有一个Action时，这个Action会分发到各个Model，如：上网shopping，提交订单这一个动作会分解到有一条订单要记录下来、用户账户会发生变化、优惠信息会发生变化，物流信息会发生变化，会影响库存等。这么多Model都要更新，更新后页面上的多个View也会跟着变化，互相之间也会有影响，可能系统的状态会变得不可预测，不知道下单后页面会发生什么变化。

####  2）Flux
用户会有各种各样的Action（行为），所有的行为由一个统一的Dispatcher去分发，一个Action只能分发到若干个Store里面，Store保持着数据，也保存着当前页面的状态，根据用户的行为以及页面当前的状态，一个Store只能向视图层传递信息，不允许视图层反回来作用在Store上，视图就会发生变化，再由用户去传入新的Action，这样数据的流向是单向的。

###  5、Redux概述
Redux其实是Flux框架的一种实现方法。Redux与Flux的思想有点不同。

大概的过程：当一个页面渲染完后，UI出现，用户其实是触发了UI上的一些Action，Action将会被送到Reducers方法里，Reducers将会更新Store，数据就是React开发中的State，State其实是Store的一部分，所有的视图层的东西，也就是组件，其实是由State来唯一决定的。

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1440780/1594604634659-2943db52-ffc0-4813-aad7-6fd49e0e2e3c.jpeg)

####  6、React概述
#### React可以认为有以下三部分组成：
#### 1）React.createClass()：

创建一个React组件
属性this.props用于父子组件之间传递属性；</br>
属性this.state用于表示组件本身的状态，当state变化时，该组件会根据最新的state正确渲染出来；</br>
方法render()中定义组件渲染成什么样子，应该是怎么样的一种呈现，在其中写JSX；</br>
方法需要了解生命周期。</br>

#### 2）ReactDOM.render()：
把组件（包括嵌套组件）渲染到页面的某个节点上。</br>
ReactDOM是在0.14版本之后才拆出来的，在之前是只有React，统称为React。</br>

#### 3）各种属性
一般不怎么用到，在此不作叙述。

####  13.  Redux如何实现多个组件之间的通信，多个组件使⽤用相同状态如何进⾏管理

请使用状态提升的方式在多个组件之间共享数据

切记维持应用单向数据流和数据唯一来源原则。

####  例子
Redux的特点
* 统一的状态管理，一个应用中只有一个仓库（store）
* 仓库中管理了一个状态树（statetree）
* 仓库不能直接修改，修改只能通过派发器（dispatch）派发一个动作（action）
* 更新state的逻辑封装到reducer中

随着JavaScript单页应用开发日趋复杂，管理不断变化的state非常困难，Redux的出现就是为了解决state里的数据问题。在React中，数据在组件中是单向流动的，数据从一个方向父组件流向子组件(通过props)，由于这个特征，两个非父子关系的组件（或者称作兄弟组件）之间的通信就比较麻烦

####  Redux能做什么？

随着JavaScript单页应用开发日趋复杂，管理不断变化的state非常困难，Redux的出现就是为了解决state里的数据问题。在React中，数据在组件中是单向流动的，数据从一个方向父组件流向子组件(通过props)，由于这个特征，两个非父子关系的组件（或者称作兄弟组件）之间的通信就比较麻烦

####  redux中各对象的说明
####  store
store是一个数据仓库，一个应用中store是唯一的，它里面封装了state状态，当用户想访问state的时候，只能通过store.getState来取得state对象，而取得的对象是一个store的快照，这样就把store对象保护起来。

###  action
action描述了一个更新state的动作，它是一个对象，其中type属性是必须有的，它指定了某动作和要修改的值：

`{type: UPDATE_TITLE_COLOR, payload: 'green'}`

###  actionCreator
如果每次派发动作时都写上长长的action对象不是很方便，而actionCreator就是创建action对象的一个方法，调用这个方法就能返回一个action对象，用于简化代码。

####  dispatch
dispatch是一个方法，它用于派发一个动作action，这是唯一的一个能够修改state的方法，它内部会调用reducer来调用不同的逻辑基于旧的state来更新出一个新的state。

###  reducer
reducer是更新state的核心，它里面封装了更新state的逻辑，reducer由外界提供（封装业务逻辑，在createStore时传入），并传入旧state对象和action，将新值更新到旧的state对象上返回。
####  使用redux的流程

####  1.定义动作类型：

const INCREAMENT='INCREAMENT';

####  2.定义项目的默认状态，传入reducer
```javascript
let initState={...};
function reducer(state=initState,action){
    //...
}
```
####  3.编写reducer，实现更新state的具体逻辑
```javascript
function reducer(state=initState,action){
    let newState;
    switch(action.type){
        //...
    }
    return newState;
}
```
###  4.创建容器，传入reducer
```javascript
let store=createStore(reducer);
```

订阅需要的方法，当state改变会自动更新
```javascript
store.subcribe(function(){

});
```
在需要更新state的地方调用dispatch即可
```javascript
store.dispatch(/*某个action*/);
```

可以看到通过以上几个步骤，就可以使用redux，且不局限于某种“框架”中，redux是一个设计思想，只要符合你的需求就可以使用redux。

###  在React中使用Redux
>以下编写一个待办事项的小功能，描述如下：
* 可以让用户添加待办事项（todo）
* 可以统计出还有多少项没有完成
* 用户可以勾选某todo置为已完成
* 可筛选查看条件（显示全部、显示已完成、显示未完成）

###  小项目的目录结构：
###  项目根结点
项目根结点
![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2010201633_1603182790098.png)

以上4个功能我们使用redux结合react来实现。</br>

####  组件拆分为3个：
* TodoHeader 用于展示未办数量和添加待办
* TodoList 按条件展示待办项列表
* TodoFooter 功能按钮（显示全部、未完成、已完成）

####  统计未完成的事项
此功能的核心就是把所有“未完成”的数量统计出来，在编写redux程序时，首先定义好默认state，默认state是写在reducer中的：
//定义默认状态
```javascript
let initState = {
  todos: [
    {
      id: parseInt(Math.random() * 10000000),
      isComplete: false,
      title: '学习redux'
    }, {
      id: parseInt(Math.random() * 10000000),
      isComplete: true,
      title: '学习react'
    }, {
      id: parseInt(Math.random() * 10000000),
      isComplete: false,
      title: '学习node'
    }
  ]
};
```
在reducer目录下创建一个index.js，由于这4个功能点过于简单，不必拆分为多个reducer，因此所有的功能都在这一个index.js文件中完成。

这样还可以减少combineReducer这个步骤。

以上定义了一个默认state对象，它里面有3条数据，描述了待办事项的内容。
由于目前没有具体的功能逻辑，我们创建一个空的reducer：
```javascript
function reducer(state=initState,action){
  return state;
}
export default reducer;
```
可以看到，传入了默认的initState，这样就可以基于旧的state对象来作更新，每次reducer都会根据原state更新出一个新的state返回。
之后就可以创建仓库（store），引用刚刚写好的reducer，并把store返回给顶层组件使用：
```javascript
import {createStore} from 'redux';
import reducer from './reducers';

let store = createStore(reducer);//传入reducer
export default store;
```
在store目录下的index.js默认导出store对象，方便组件引入。

在根组件中引入store对象，它是所有组件的容器，因此它要做所有组件的store提供者的角色，所以它的任务要把store提供给所有子组件使用，这就需要react-redux包提供的一个组件：Provider：
Provider也是一个组件，它只有一个属性：store，传入创建好的store对象即可：
```javascript
import {Provider} from 'react-redux';
import store from '../store';
//其它代码略...
ReactDOM.render(<Provider store={store}>
  <div>
    <TodoHeader/>
    <TodoList/>
    <TodoFooter/>
  </div>
</Provider>, document.querySelector('#root'));
```
这样就意味着Provider包裹的所有组件都可合法的取到store。
现在数据已经提供，还需要子组件来接收，同样接收store数据react-redux包也为我们提供了一个方法：connect。

connect这个方法非常奇妙，它的功能非常强大，它可以把仓库中state数据注入到组件的属性（this.props）中，这样子组件就可以通过属性的方式拿到仓库中的数据。
首先定义一个头组件，用于显示未完成的数量：
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class TodoHeader extends React.Component {
//代码略...
}
下面使用connect方法将state数据注入到TodoHeader组件中：
import {connect} from 'react-redux';
let ConnectedTodoHeader = connect((state) => ({
  ...state
}))(TodoHeader);
```
可以看到它的写法很怪，connect是一个高阶函数（函数返回函数），它的最终返回值是一个组件，这个组件（ConnectedTodoHeader）最终“连接”好了顶层组件Provider提供的store数据。

connect的第一个参数是一个函数，它的返回是一个对象，返回的对象会绑定到目标组件的属性上，函数参数state就是store.getState的返回值，使用它就可以取到所有state上的数据，目前state就是todos的3条待办事项

而高阶函数传入的参数就是要注入的组件，这里是TodoHeader，这样在TodoHeader组件中就可以通过this.props.todos取到待办事项的数据。

这样就可以编写好我们的第一个统计功能，下面附上代码：
```javascript
class TodoHeader extends React.Component {
  //取得未完成的todo数量
  getUnfinishedCount() {
    //this.props.todos就是从connect传入的state数据
    return this.props.todos.filter((i) => {
      return i.isComplete === false;
    }).length;
  }
  render() {
    return (<div>
      <div>您有{this.getUnfinishedCount()}件事未完成</div>
    </div>);
  }
}

//导出注入后的组件
export default connect((state) => ({
  ...state//此时的state就是todos:[...]数据
}))(TodoHeader);
```

可以看到，通过connect取得state注入到组件属性上，即可编写逻辑完成功能。
####  添加待办项
接下来完成添加待办项的功能，用户在一个文本框中输入待办项，把数据添加到仓库中，并更新视图。
由于有用户的操作了，我们需要编写动作（Action），Action需要一个具体的动作类型，我们在action-types.js中创建需要动作类型即可：
```javascript
//添加待办事项
export const ADD_TODO = 'ADD_TODO';
```
可以看到它非常简单，就定义了一个动作类型，也就是一个描述Action动作的指令，导出它给reducer来使用。

接下来编写ActionCreator，它是一个函数，只返回用刚刚这个指令生成的Action对象：
```javascript
import {ADD_TODO} from './action-type/action-types';

let actions = {
  addTodo: function(payload) {
    return {type: ADD_TODO, payload};
  }
};

export default actions;//导出ActionCreators
```
可以看到引入了action-type，addTodo返回了一个形如{type:XXX, payload:XXX}的一个Action对象。这就是一个标准的Action对象的形式，第二个参数payload就是用户传入的参数。

注意在导出时一定要将ActionCreator函数包到一个对象中返回，这样redux内部会通过bindActionCreators将dispatch的功能封装到每个函数中，这样在connect连接时极大的方便了用户的操作，稍候会看到。

下面编写reducer，它里面封装了“添加待办项”的逻辑：
```javascript
import {ADD_TODO} from '../actions/action-type/action-types';
//部分代码略...
function reducer(state = initState, action) {
  let newState;
  switch (action.type) {
    case ADD_TODO:
      newState = {
        todos: [
          ...state.todos,
          action.payload
        ]
      };
      break;
    default:
      newState = state;
      break;
  }
  return newState;
}
```
以上通过switch语句的一个分支，判断动作类型是不是“添加待办”这个功能（ADD_TODO），这样在原state对象的基础上追加这条数据即可。

注意，每次reducer都返回一个新的对象，不要直接在原state.todos.push这条数据，因为reducer是一个纯函数。

...是ES6的写法，意为展开运算符，它是将原state.todos的数据展开，并在后面添加一条新数据，相当于合并操作。

好了，到此处理数据的部分已经写好，又到了注入组件的工作了，创建展示待办的组件TodoList：
```javascript
import React from 'react';
import {connect} from 'react-redux';
class TodoList extends React.Component {
//代码略...
}
export default connect((state) => ({
  ...state
}))(TodoList);
```
再次通过connect方法将state数据注入到组件 （TodoList）的属性上，让组件内部可以通过this.props取得state数据。
###  下面编写展示待办项的功能：
```javascript
class TodoList extends React.Component {
  getTodos() {
    return this.props.todos.map((todo, index) => {
      return (<li key={index}>
        <input type="checkbox" checked={todo.isComplete}/> {
          todo.isComplete
            ? <del>{todo.title}</del>
            : <span>{todo.title}</span>
        }
        <button type="button" data-id={todo.id}>删除</button>
      </li>);
    });
  }
  render() {
    return (<div>
      <ul>
        {this.getTodos()}
      </ul>
    </div>);
  }
}
```
在组件中定义一个getTodos方法用于循环所有待办项，可以看到通过this.props.todos即可拿到connect传入的数据，并在render中调用getTodos渲染即可。

现在可以初探整个小项目的逻辑，我们取数据不再是通过一层一层的组件传递了，而是所有的数据操作都交由redux来解决，组件只负责展示数据。

####  更改待办项状态
接下来实现更改一条待办项的状态，当用户给一条待办打勾就记为已完成，否则置为未完成。
还是一样，新建一个action-type：
```javascript
//更改待办项的完成状态
export const TOGGLE_COMPLETE = 'TOGGLE_COMPLETE';
创建actionCreator，引入这个action-type：
let actions = {
  //更改完成状态，此处payload传id
  toggleComplete: function(payload) {
    return {type: TOGGLE_COMPLETE, payload};
  }
    //其它略...
};
由于用户勾选一条记录，应传入id作为唯一标识，因此这里的payload参数就是待办项的id。
payload并不是一定要叫payload可以更改变量名，如todoId，redux中管个这变量叫载荷，因此这里使用payload。
同样在reducer中再加一个swtich分支，判断TOGGLE_COMPLETE：
function reducer(state = initState, action) {
  let newState;
  switch (action.type) {
    case TOGGLE_COMPLETE:
      newState = {
        //循环每一条待办，把要修改的记录更新
        todos: state.todos.map(item => {
          if (item.id == action.payload) {
            item.isComplete = !item.isComplete;
          }
          return item;
        })
      };
      break;
    //其它代码略...
    default:
      newState = state;
      break;
  }
  return newState;
}
可以看到这次是修改某一条记录的isComplete属性，因此使用map函数循环，找到id为action.payload的那一条，修改isComplete的状态。

仍要注意，不要使用slice函数去修改原state，一定要返回一个基于state更新后的新对象，map函数的执行结果就是返回一个新数组，因此使用map符合这里的需求。
接下来为组件的checkbox元素添加事件，当用户勾选时，调用对应的Action toggleComplete动作即可完成逻辑：
//引入actionCreators
import actions from '../store/actions';
//其它 代码略...
class TodoList extends React.Component {
  todoChange = (event) => {
    //当onChange事件发生时，调用toggleComplete动作
    this.props.toggleComplete(event.target.value);
  }
  getTodos() {
    return this.props.todos.map((todo, index) => {
      return (<li key={index}>
        <input type="checkbox" value={todo.id} checked={todo.isComplete} onChange={this.todoChange}/> {
          todo.isComplete
            ? <del>{todo.title}</del>
            : <span>{todo.title}</span>
        }
        <button type="button" data-id={todo.id}>删除</button>
      </li>);
    });
  }
  render() {
    //略...
  }
}
export default connect((state) => ({
  ...state
}), actions)(TodoList); //第二个参数传入actionCreators
```

这里的connect函数传入了第二个参数，它是一个actionCreator对象，同理由于组件中需要调用Action派发动作以实现某个逻辑，比如这里就是组件需要更新待办项的状态，则“功能”也是由redux传给组件的。

这样组件里的this.props就可以拿到actionCreator的方法，以调用逻辑：
```javascript
this.props.toggleComplete()。
```
现在可以看到connect函数的强大之处，不管是数据state和功能actionCreators，都是由redux传给需要调用的组件。redux在内部自动处理了更新组件、数据传递的工作，我们开发者不必再为组件之间的通信花费精力了。

我们的今后的工作就是按照redux的架构定义好动作（Action）和reducer，也就是业务逻辑，而其它繁复的工作都由redux来完成。

删除待办项的功能类似，不再详述。
###  筛选查看条件
筛选查看条件需要预先定义好3个状态，即查看全部（all）只查看未完成（uncompleted）和查看已完成（completed）。
因此，我们修改初始化的状态，让它默认为“查看全部”：
```javascript
//定义默认状态
let initState = {
    //display用于控制待办项列表的显示
  display:'all', 
  todos: [
  //略...
  ]
};
```
同样的套路，创建action-type：
```javascript
//更改显示待办项的状态
export const CHANGE_DISPLAY = 'CHANGE_DISPLAY';
```
###  创建actionCreator：
```javascript
//部分代码略...
let actions = {
  //更改显示待办项的状态，
  //payload为以下3个值（all,uncompleted,completed）
  changeDisplay: function(payload) {
    return {type: CHANGE_DISPLAY, payload};
  }
};
```
为reducer增加CHANGE_DISPLAY的逻辑：
```javascript
//部分代码略...
function reducer(state = initState, action) {
  let newState;
  switch (action.type) {
    case CHANGE_DISPLAY:
      newState = {
        display: action.payload,
        todos: [...state.todos]
      };
      break;
    default:
      newState = state;
      break;
  }
  return newState;
}
```

在组件中，根据display条件过滤待办项的数据即可，这里抽出一个方法filterDisplay来实现：
```javascript
class TodoList extends React.Component {
  //按display条件过滤数据
  filterDisplay() {
     return this.props.todos.filter(item => {
      switch (this.props.display) {
        case 'completed':
          return item.isComplete;
        case 'uncompleted':
          return !item.isComplete;
        case 'all':
        default:
          return true;
      }
    });
  }
  getTodos() {
    return this.filterDisplay().map((todo, index) => {
      //略...
    });
  }
  render() {
    //略...
  }
}
export default connect((state) => ({
  ...state
}), actions)(TodoList);
```
以上还是由connect方法注入数据到组件，根据状态的display条件过滤出符合条件的数据即可。
到此，全部的功能已实现。

###  运行效果：
这个例子虽简单却完整的展示了redux的使用，真正项目开发时只要遵循redux的“套路”即可。
要需了解redux的更深层逻辑原理，就要读redux的源码，其实也并不复杂。


###  65.多个组件之间如何拆分各⾃的`state`，每块小的组件有自己的状态，它们之间还有⼀些公共的状态需要维护，如何思考这块?
* 1.如果状态具有复杂的更新逻辑，则将该逻辑从组件中提取到自定义钩子中。
* 2.保证唯一数据源和单向数据
* 3.将复杂的状态逻辑提取到自定义钩子中(自定义useReducer)。
* 4.对于组件的拆分要做到高内聚低耦合