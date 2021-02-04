#### 1.`vue`优点？

#### 轻量级框架：
只关注视图层，是一个构建数据的视图集合，大小只有几十kb；
#### 简单易学：
国人开发，中文文档，不存在语言障碍 ，易于理解和学习； 双向

#### 数据绑定：
保留了angular的特点，在数据操作方面更为简单； 
#### 组件化：
保留了react的优点，实现了html的封装和重用，在构建单页面应用方面有着独特的优势； 
视图，数据，结构分离：
使数据的更改更为简单，不需要进行逻辑代码的修改，只需要操作数据就能完成相关操作； 
#### 虚拟DOM：
dom操作是非常耗费性能的， 不再使用原生的dom操作节点，极大解放dom操作，但具体操作的还是dom不过是换了另一种方式； 
运行速度更快:
相比较与react而言，同样是操作虚拟dom，就性能而言，vue存在很大的优势。

#### 2.`vue`父组件向子组件传递数据？
1.通过`props`

2.通过父组件　//触发子组件方法，并传参
```
this.$refs.mychild.init("chrchr","120");
```

####  3.子组件向父组件传递事件?

### $emit方法

#### 自定义子组件
```html
<template>
  <div class="container">
     <p @click="clickAction">{{titleName}}</p>
    <div class="line"></div>
  </div>
</template>
 ```

 ```javascript
export default {
  name: "HelloWorld",
  props:{
    titleName:{
      type:string,
      default:""
    }
  },
  methods: {
    clickAction(){
      this.$emit('clickChild',this.titleName);
    }
  }
};
```

#### 父组件中调用子组件
 ```html
<div class="message">
   <ActivityHead 
   :titleName="msgRight" 
   @clickChild="clickChild">
   </ActivityHead>
 </div>
 ```
 
 ```javascript
import ActivityHead from "./ActivityHead.vue";
export default {
  name: "HelloWorld",
  components: {
    ActivityHead
  },
  methods: {
      clickChild(msg){
       console.log(msg);
      }
  }
};
```


#### 4.`v-if`与`v-show`的区别

#### 共同点：

都能控制元素的显示和隐藏； 

#### 不同点：

实现本质方法不同，v-show本质就是通过控制css中的display设置为none，控制隐藏，只会编译一次；v-if是动态的向DOM树内添加或者删除DOM元素，若初始值为false，就不会编译了。

而且v-if不停的销毁和创建比较消耗性能。 总结：如果要频繁切换某节点，使用v-show(切换开销比较小，初始开销较大)。

如果不需要频繁切换某节点使用v-if（初始渲染开销较小，切换开销比较大）。

#### 5.`v-if` 与 `v-for`的优先级?

#### 核心答案：
1、v-for优先于v-if被解析
2、如果同时出现，每次渲染都会先执行循环再判断条件，无论如何循环都不可避免，浪费了性能
3、要避免出现这种情况，则在外层嵌套template，在这一层进行v-if判断，然后在内部进行v-for循环
4、如果条件出现在循环内部，可通过计算属性提前过滤掉那些不需要显示的项

#### 6.`vue`组件中`data`为什么必须是一个函数？

如果`data`是一个函数的话，这样每复用一次组件，就会返回一份新的`data`，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。

而单纯的写成对象形式，就使得所有组件实例共用了一份`data`，就会造成一个变了全都会变的结果。

所以说`vue`组件的`data`必须是函数。这都是因为`js`的特性带来的，跟vue本身设计无关。

`js`本身的面向对象编程也是基于原型链和构造函数，应该会注意原型链上添加一般都是一个函数方法而不会去添加一个对象了。


#### 7.`VueX`之`actions`与`mutations`的区别?

#### actions

1、用于通过提交mutation改变数据

2、会默认将自身封装为一个`Promise`

3、可以包含任意的异步操作

#### mutations

1、通过提交`commit`改变数据

2、只是一个单纯的函数

3、不要使用异步操作，异步操作会导致变量不能追踪

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly92dWV4LnZ1ZWpzLm9yZy92dWV4LnBuZw?x-oss-process=image/format,png)


#### 2.如何在vuex中使用异步修改?

在调用`vuex`中的方法`action`的时候，用`promise`实现异步修改

```JAVASCRIPT
const actions = {
    asyncLogin({ commit }, n){
        return new Promise(resolve => {
            setTimeout(() => {
                commit(types.UserLogin, n);
                resolve();
            },3000)
        })
    }
}
```

#### 8.`Vue`有哪些组件间的通信方式?

### 核心答案：
>Vue 组件间通信只要指以下 3 类通信：
>父子组件通信、隔代组件通信、兄弟组件通信，下面我们分别介绍每种通信方式且会说明此种方法可适用于哪类组件间通信。

### 方法一
```js
props/$emit
```
父组件`A`通过`props`的方式向子组件`B`传递，`B to A` 通过在 B 组件中 `$emit`, `A` 组件中 `v-on` 的方式实现。


#### 1.父组件向子组件传值

接下来我们通过一个例子，说明父组件如何向子组件传递值：在子组件Users.vue中如何获取父组件App.vue中的数据 
```js
userList:["Henry","Bucky","Emily"]
```
```html
//App.vue父组件
<template>
  <div id="app">
    <hook-users 
    :userList="userList"/>
    //前者自定义名称便于子组件调用，后者要传递数据名
  </div>
</template>
<script>
import AppUsers from "./Components/AppUsers"
export default {
  name: 'App',
  data(){
    return{
      userList:["Henry","Bucky","Emily"]
    }
  },
  components:{
    "app-users":AppUsers
  }
}
```
```html
//users子组件
<template>
  <div class="hello">
    <ul>
      //遍历传递过来的值，然后呈现到页面
      <li v-for="user in userList">{{user}}</li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'AppUsers',
  props:{
    userList:{ 
      type:Array,
      required:true
    }
  }
}
</script>
```

#### 总结：
父组件通过`props`向下传递数据给子组件。注：组件中的数据共有三种形式：`data`、`props`、`computed`


#### 2.子组件向父组件传值（通过事件形式）
```html
// 子组件
<template>
  <header>
    <h1 @click="changeTitle">{{title}}</h1>//绑定一个点击事件
  </header>
</template>
<script>
export default {
  name: 'AppHeader',
  data() {
    return {
      title:"Vue.js Demo"
    }
  },
  methods:{
    changeTitle() {
      this.$emit("titleChanged","子向父组件传值");
      //自定义事件  传递值“子向父组件传值”
    }
  }
}
</script>

// 父组件
<template>
  <div id="app">
    <app-header v-on:titleChanged="updateTitle" ></app-header>
    //与子组件titleChanged自定义事件保持一致
    // updateTitle($event)接受传递过来的文字
    <h2>{{title}}</h2>
  </div>
</template>
<script>
import Header from "./components/Header"
export default {
  name: 'App',
  data(){
    return{
      title:"传递的是一个值"
    }
  },
  methods:{
    updateTitle(e){   //声明这个函数
      this.title = e;
    }
  },
  components:{
   "app-header":Header,
  }
}
</script>
```

#### 总结：

子组件通过`events`给父组件发送消息，实际上就是子组件把自己的数据发送到父组件。


### 方法二、$emit/$on

这种方法通过一个空的`Vue`实例作为中央事件总线（事件中心），用它来触发事件和监听事件,巧妙而轻量地实现了任何组件间的通信，包括父子、兄弟、跨级。当我们的项目比较大时，可以选择更好的状态管理解决方案`vuex`。

1.具体实现方式：
```javascript
var App=new Vue();
App.$emit(事件名,数据);
App.$on(事件名,data => {});
```

或者自己实现一个
```javascript
class MyEventEmitter {
  constructor() {
    this.event = {};
  }
  // 监听
  on(type, listener) {
    if (this.event[type]) {
      this.event[type].push(listener);
    } else {
      this.event[type] = [listener];
    }
  }
  //发送监听
  emit(type, ...rest) {
    if (this.event[type]) {
      this.event[type].map(fn => fn.apply(this, rest));
    }
  }
  //移除监听器
  removeListener(type) {
    if (this.event[type]) {
      delete this.event[type];
      console.log(this.event);
    }
  }
  //移除所有的监听器
  removeAllListener() {
    this.event = {};
  }
}


var MyEvent=new MyEventEmitter();
MyEvent.$emit(事件名,数据);
MyEvent.$on(事件名,data => {});

```
>但是这种方式，记得在每次触发监听的时候，记得移除上一个监听器

### 方法三、Vuex与localStorage

vuex 是 vue 的状态管理器，存储的数据是响应式的。但是并不会保存起来，刷新之后就回到了初始状态，具体做法应该在vuex里数据改变的时候把数据拷贝一份保存到localStorage里面，刷新之后，如果localStorage里有保存的数据，取出来再替换store里的state。
```javascript
const jsonToString=(json)=>{
  return JSON.stringify(json)
}

const stringToJson=(keyName)=>{
  //暂不验证数据格式
   return window.localStorage.getItem(keyName)?
   JSON.parse(window.localStorage.getItem(keyName))
   :{};
}
export default new Vuex.Store({
  state: {
    selectCity:stringToJson("selectCity")
  },
  mutations: {
    changeCity(state, selectCity) {
      state.selectCity = selectCity
      try {
        window.localStorage.setItem('selectCity',jsonToString(state.selectCity));
      // 数据改变的时候把数据拷贝一份保存到localStorage里面
      } catch (e) {}
    }
  }
})

```

### 方法四、$attrs/$listeners

#### 如图:
![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2009171600_1600329633808.png)

#### 场景

有些变态需求：比如说A父组件里面导入了B组件，可是B组件里面又导入了C组件，现在需要A父组件传值给C组件，或者是C组件需要传值给父组件，这时候就需要用到$attrs和$listeners了。

#### $attrs
包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。（父传孙专用）

#### $listener

包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件——在创建更高层次的组件时非常有用。（孙传父专用）

在父组件当中，最外层组件

```html
<template>
    <div>
        <Child1 
        :child1Info="child1" 
        :child2Info="child2" 
        v-on:test1="onTest1" 
        v-on:test2="onTest2">
        </Child1>
    </div>
</template>
<script>
import Child1 from './child1';
export default {
    data() {
        return {
            child1:"hahha",
            child2:"asdsdasd"
        };
    },
    components: { Child1 },
    methods: {
        onTest1(msg) {
            console.log('test1 running...',msg);
        },
        onTest2(msg) {
            console.log('test2 running',msg);
        }
    }
};
</script>
···

//在子组件中
```html
<template>
    <div class="child-1">
        <p>在子组件当中:</p>
        <p>props-child1Info: {{child1Info}}</p>
        <p>$attrs: {{$attrs}}</p>
        <hr>
        <!-- Child2组件中能直接触发test的原因在于 B组件调用C组件时 使用 v-on 绑定了$listeners 属性 -->
        <!-- 通过v-bind 绑定$attrs属性，Child2组件可以直接获取到A组件中传递下来的props（除了child1组件中props声明的） -->
        <Child2 v-bind="$attrs" v-on="$listeners"></Child2>
    </div>
</template>
<script>
import Child2 from './child2';
export default {
    props: ['child1Info'],
    data() {
        return {};
    },
    components: { Child2 },
    mounted() {
        this.$emit('test1','嘻嘻');
    }
};
</script>
```

//在孙子组件当中：
```html
<template>
    <div class="child-2">
        <p>在最里层组件当中child2:</p>
        <p>props-child2Info: {{child2Info}}</p>
        <p> $attrs 的值: {{$attrs}}</p>
        <hr>
    </div>
</template>
<script>
export default {
    props: ['child2Info'],
    data() {
        return {};
    },
    mounted() {
        this.$emit('test2','哈哈');
    }
};
</script>
```

#### 代码详细说明：

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2009171558_1600329518020.png)

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2009171558_1600329528854.png)

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2009171600_1600329603285.png)


#### 9.`Vue`中双向数据绑定是如何实现的?

1.`vue.js` 则是采用数据劫持结合发布者-订阅者模式的方式。

2.通过`Object.defineProperty()`来劫持各个属性的`setter`，`getter`.

3.在数据变动时发布消息给订阅者，触发相应的监听回调。我们先来看`Object.defineProperty()`这个方法：

```javascript
var obj  = {};
Object.defineProperty(obj, 'name', {
        get: function() {
            console.log('我被获取了')
            return val;
        },
        set: function (newVal) {
            console.log('我被设置了')
        }
})
obj.name = 'fei';
//在给obj设置name属性的时候，触发了set这个方法
var val = obj.name;
//在得到obj的name属性，会触发get方法
```

#### 10.单页面应用和多页面应用区别及优缺点?

单页面应用（`SPA`），通俗一点说就是指只有一个主页面的应用，浏览器一开始要加载所有必须的 `html`, `js`, `css`。所有的页面内容都包含在这个所谓的主页面中。但在写的时候，还是会分开写（页面片段），然后在交互的时候由路由程序动态载入，单页面的页面跳转，仅刷新局部资源。多应用于`pc`端。

多页面（`MPA`），就是指一个应用中有多个页面，页面跳转时是整页刷新

#### 单页面的优点：

1，用户体验好，快，内容的改变不需要重新加载整个页面，基于这一点spa对服务器压力较小

2，前后端分离

3，页面效果会比较炫酷（比如切换页面内容时的专场动画）

#### 单页面缺点：

1，不利于`seo`

2，导航不可用，如果一定要导航需要自行实现前进、后退。（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）

3，初次加载时耗时多

4，页面复杂度提高很多

#### 11.`vue`中`v-if`和`v-for`优先级?

`v-for`和`v-if`不应该一起使用，必要情况下应该替换成`computed`属性。原因：`v-for`比`v-if`优先，如果每一次都需要遍历整个数组，将会影响速度，尤其是当之需要渲染很小一部分的时候。
```html
<li
  v-for="user in users"
  v-if="user.isActive"
  :key="user.id">
  {{ user.name }}
</li>
```

如上情况，即使100个user中之需要使用一个数据，也会循环整个数组。

```javascript
omputed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}
```

```html
<ul>
  <li
  v-for="user in activeUsers" 
  :key="user.id">
  {{ user.name }}
  </li>
</ul>
```

#### 12.`Vue`事件的修饰符（）？


1)``.stop`：等同于`JavaScript`中的`event.stopPropagation()``，防止事件冒泡

2)`.prevent`：等同于`JavaScript`中的`event.preventDefault()`，防止执行预设的行为（如果事件可取消，则取消该事件，而不停止事件的进一步传播）

3)`.capture`：与事件冒泡的方向相反，事件捕获由外到内

4)`.self`：只会触发自己范围内的事件，不包含子元素

5)`.once`：只会触发一次

6)`.passive`：`passive`表示`listener`函数不会调用`preventDefault（）`

`passive`主要用在移动端的`scroll`事件，来提高浏览器响应速度，提升用户体验。因为`passive=true`等于提前告诉了浏览器，`touchstart`和`touchmove`不会阻止默认事件，手刚开始触摸，浏览器就可以立刻给与响应；

否则，手触摸屏幕了，但要等待`touchstart`和`touchmove`的结果，多了这一步，响应时间就长了，用户体验也就差了。


#### 13.`Vue`的两个核心是什么？

#### 1、数据驱动：
在vue中，数据的改变会驱动视图的自动更新。传统的做法是需要手动改变DOM来使得视图更新，而vue只需要改变数据。

#### 2、组件
组件化开发，优点很多，可以很好的降低数据之间的耦合度。将常用的代码封装成组件之后（vue组件封装方法），就能高度的复用，提高代码的可重用性。一个页面/模块可以由多个组件所组成。

##### 14、`react`和`vue`的区别

#### 相同点

* 数据驱动页面提供响应式的试图组件
* 都有`virtual DOM`,组件化的开发通过`props`参数进行父子之间组件传递数据都实现了`webComponents`规范
* 数据流动单向都支持服务器的渲染SSR
* 都有支持`native`的方法`react`有`React native vue`有`wexx`

#### 不同点

* 数据绑定`Vue`实现了双向的数据绑定`react`数据流动是单向的
* 数据渲染大规模的数据渲染`react`更快
* 使用场景`React`配合`Redux`架构适合大规模多人协作复杂项目Vue适合小快的项目
* 开发风格`react`推荐做法`jsx` + `inline style`把`html`和`css`都写在`js`了
* `vue`是采用`webpack` +`vue-loader`单文件组件格式`html`, `js`, `css`同一个文件



#### 15.`vue3.0`有哪些新特性

#### vue3.0的设计目标
* 更小
* 更快
* 加强TypeScript支持
* 加强API设计一致性
* 提高自身可维护性
* 开放更多底层功能

具体可以从以下方面来理解

#### 1，压缩包体积更小
当前最小化并被压缩的 `Vue` 运行时大小约为 20kB（2.6.10 版为 22.8kB）。`Vue 3.0`捆绑包的大小大约会`减少一半`，即只有`10kB`！

#### 2，Object.defineProperty -> Proxy

`Object.defineProperty`是一个相对比较昂贵的操作，因为它直接操作对象的属性，颗粒度比较小。将它替换为`es6`的`Proxy`，在目标对象之上架了一层拦截，代理的是对象而不是对象的属性。这样可以将原本对对象属性的操作变为对整个对象的操作，颗粒度变大。

`javascript`引擎在解析的时候希望对象的结构越稳定越好，如果对象一直在变，可优化性降低，`proxy`不需要对原始对象做太多操作。

#### 3，Virtual DOM 重构

vdom的本质是一个抽象层，用`javascript`描述界面渲染成什么样子。`react`用`jsx`，没办法检测出可以优化的动态代码，所以做时间分片，`vue`中足够快的话可以不用时间分片。

#### 传统vdom的性能瓶颈：

虽然 Vue 能够保证触发更新的组件最小化，但在单个组件内部依然需要遍历该组件的整个 vdom 树。
传统 vdom 的性能跟模版大小正相关，跟动态节点的数量无关。在一些组件整个模版内只有少量动态节点的情况下，这些遍历都是性能的浪费。
`JSX` 和手写的 `render function` 是完全动态的，过度的灵活性导致运行时可以用于优化的信息不足
那为什么不直接抛弃vdom呢？

高级场景下手写 `render function` 获得更强的表达力

生成的代码更简洁

#### 兼容2.x

`vue`的特点是底层为`Virtual DOM`，上层包含有大量静态信息的模版。为了兼容手写 `render function`，最大化利用模版静态信息，`vue3.0`采用了动静结合的解决方案，将`vdom`的操作颗粒度变小，每次触发更新不再以组件为单位进行遍历，主要更改如下

将模版基于动态节点指令切割为嵌套的区块

每个区块内部的节点结构是固定的

每个区块只需要以一个 Array 追踪自身包含的动态节点

vue3.0将 vdom 更新性能由与模版整体大小相关提升为与动态内容的数量相关


#### 4, 更多编译时优化
Slot 默认编译为函数：父子之间不存在强耦合，提升性能
Monomorphic vnode factory：参数一致化，给它children信息，
Compiler-generated flags for vnode/children types

#### 5，选用Function_based API
为什么撤销 `Class API` ?

1，更好地支持`TypeScript`

`Props` 和其它需要注入到 `this` 的属性导致类型声明依然存在问题
`Decorators`提案的严重不稳定使得依赖它的方案具有重大风险
2，除了类型支持以外 `Class API` 并不带来任何新的优势

3，`vue`中的`UI`组件很少用到继承，一般都是组合，可以用`Function-based API`


#### 16.`Vue`性能优化方法

### 1）编码阶段
* 尽量减少data中的数据，data中的数据都会增加getter和setter，会收集对应的watcher；
* 如果需要使用v-for给每项元素绑定事件时使用事件代理；
* SPA 页面采用keep-alive缓存组件；
* 在更多的情况下，使用v-if替代v-show；
* key保证唯一；
* 使用路由懒加载、异步组件；
* 防抖、节流；
* 第三方模块按需导入；
* 长列表滚动到可视区域动态加载；
* 图片懒加载；

### 2）用户体验：
* 骨架屏；
* PWA；
* 还可以使用缓存(客户端缓存、服务端缓存)优化、服务端开启gzip压缩等。

### 3）SEO优化
* 预渲染；
* 服务端渲染SSR；

### 4）打包优化
* 压缩代码；
* Tree Shaking/Scope Hoisting；
* 使用cdn加载第三方模块；
* 多线程打包happypack；
* splitChunks抽离公共文件；
* sourceMap优化；


#### 17.`v-model`的原理

`v-model`本质就是一个语法糖，可以看成是value + input方法的语法糖。可以通过model属性的prop和event属性来进行自定义。

>原生的v-model，会根据标签的不同生成不同的事件和属性。

>v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

1）text 和 textarea 元素使用 value 属性和 input 事件；

2）checkbox 和 radio 使用 checked 属性和 change 事件；

3）select 字段将 value 作为 prop 并将 change 作为事件。

### 例子
```js
model: { 
  prop: 'checked', 
  event: 'change' 
}
```
如果想要更改 `checked` 这个 `prop` 可以在 `Vue` 的 `instance` 中用以下这行代码发送 `change` 这个 `event`，并将目标的变动值传给 `checked` 这个 `prop`。
```js
this.$emit('change', $event.target.value);
```


#### 18.`nextTick`的实现原理是什么?

在下次 `DOM` 更新循环结束之后执行延迟回调。`nextTick`主要使用了宏任务和微任务。根据执行环境分别尝试采用

* Promise
* MutationObserver
* setImmediate
* 如果以上都不行则采用setTimeout

定义了一个异步方法，多次调用`nextTick`会将方法存入队列中，通过这个异步方法清空当前队列。


#### 19.谈谈`Computed`和`Watch`!

`Computed`本质是一个具备缓存的`watcher`，依赖的属性发生变化就会更新视图。

适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。

`Watch`没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。

当我们需要深度监听对象中的属性时，可以打开`deep：true`选项，这样便会对对象中的每一项进行监听。

这样会带来性能问题，优化的话可以使用字符串形式监听，如果没有写到组件中，不要忘记使用`unWatch`手动注销哦。

#### 20.说一下`Vue`的生命周期

`beforeCreate`是`new Vue()`之后触发的第一个钩子，在当前阶段`data`、`methods`、`computed`以及`watch`上的数据和方法都不能被访问。

`created`在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发updated函数。可以做一些初始数据的获取，在当前阶段无法与Dom进行交互，如果非要想，可以通过`vm.$nextTick`来访问`Dom`。

`beforeMount`发生在挂载之前，在这之前`template`模板已导入渲染函数编译。而当前阶段虚拟`Dom`已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发`updated`。
`mounted`在挂载完成后发生，在当前阶段，真实的`Dom`挂载完毕，数据完成双向绑定，可以访问到`Dom`节点，使用`$refs`属性对`Dom`进行操作。

`beforeUpdate`发生在更新之前，也就是响应式数据发生更新，虚拟`dom`重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。

`updated`发生在更新完成之后，当前阶段组件`Dom`已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。
`beforeDestroy`发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

`destroyed`发生在实例销毁之后，这个时候只剩下了`dom`空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。




### 21.`Vue`模版编译原理知道吗，能简单说一下?

简单说，`Vue`的编译过程就是将`template`转化为`render`函数的过程。会经历以下阶段：

* 生成AST树
* 优化
* codegen

首先解析模版，生成`AST`语法树(一种用`JavaScript`对象的形式来描述整个模板)。
使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。

`Vue`的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的`DOM`也不会变化。那么优化过程就是深度遍历AST树，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。

编译的最后一步是将优化后的`AST`树转换为可执行的代码。

### 22.`Vue`的优点及缺点?
首先Vue最核心的两个特点，响应式和组件化。

### 响应式：
这也就是vue.js最大的优点，通过MVVM思想实现数据的双向绑定，通过虚拟DOM让我们可以用数据来操作DOM，而不必去操作真实的DOM，提升了性能。且让开发者有更多的时间去思考业务逻辑。

### 组件化：
把一个单页应用中的各个模块拆分到一个个组件当中，或者把一些公共的部分抽离出来做成一个可复用的组件。所以组件化带来的好处就是，提高了开发效率，方便重复使用，使项目的可维护性更强。

### 虚拟DOM：
当然，这个不是vue中独有的。

### 缺点：
基于对象配置文件的写法，也就是options写法，开发时不利于对一个属性的查找。另外一些缺点，在小项目中感觉不太出什么，vuex的魔法字符串，对ts的支持。兼容性上存在一些问题。

### 不利于seo:
导航不可用，如果一定要导航需要自行实现前进、后退。（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）。
初次加载时耗时多。


### 23.`Vue`中`hash`模式和`history`模式的区别?

最明显的是在显示上，`hash`模式的`URL`中会夹杂着`#`号，而history没有。

`Vue`底层对它们的实现方式不同。hash模式是依靠`onhashchange`事件(监听`location.hash`的改变)，而`history`模式是主要是依靠的`HTML5 history`中新增的两个方法，`pushState()`可以改变url地址且不会发送请求，`replaceState()`可以读取历史记录栈,还可以对浏览器记录进行修改。

当真正需要通过`URL`向后端发送HTTP请求的时候，比如常见的用户手动输入`URL`后回车，或者是刷新(重启)浏览器，这时候`history`模式需要后端的支持。

因为`history`模式下，前端的`URL`必须和实际向后端发送请求的URL一致，例如有一个`URL`是带有路径`path`的(例如`www.lindaidai.wang/blogs/id`)，如果后端没有对这个路径做处理的话，就会返回`404`错误。所以需要后端增加一个覆盖所有情况的候选资源，一般会配合前端给出的一个`404`页面。

hash:
```js
window.onhashchange = function(event){
  // location.hash获取到的是包括#号的，如"#heading-3"
  // 所以可以截取一下
  let hash = location.hash.slice(1);
}
```

#### 24.你的接口请求一般放在哪个生命周期中？

接口请求一般放在`mounted`中，但需要注意的是服务端渲染时不支持`mounted`，需要放到`created`中。


#### 25.`Vue SSR`渲染原理？

### 优点：

#### 更利于SEO

不同爬虫工作原理类似，只会爬取源码，不会执行网站的任何脚本（Google除外，据说Googlebot可以运行javaScript）。使用了Vue或者其它MVVM框架之后，页面大多数DOM元素都是在客户端根据js动态生成，可供爬虫抓取分析的内容大大减少。另外，浏览器爬虫不会等待我们的数据完成之后再去抓取我们的页面数据。服务端渲染返回给客户端的是已经获取了异步数据并执行JavaScript脚本的最终HTML，网络爬中就可以抓取到完整页面的信息。

#### 更利于首屏渲染
首屏的渲染是node发送过来的html字符串，并不依赖于js文件了，这就会使用户更快的看到页面的内容。尤其是针对大型单页应用，打包后文件体积比较大，普通客户端渲染加载所有所需文件时间较长，首页就会有一个很长的白屏等待时间。

### 场景：
交互少，数据多，例如新闻，博客，论坛类等

### 原理：

相当于服务端前面加了一层url分配，可以假想为服务端的中间层，

当地址栏url改变或者直接刷新，其实直接从服务器返回内容，是一个包含内容部分的html模板，是服务端渲染

而在交互过程中则是ajax处理操作，局部刷新，首先是在history模式下，通过history. pushState方式进而url改变，然后请求后台数据服务，拿到真正的数据，做到局部刷新，这时候接收的是数据而不是模板

### 缺点

#### 服务端压力较大
本来是通过客户端完成渲染，现在统一到服务端node服务去做。尤其是高并发访问的情况，会大量占用服务端CPU资源；

#### 开发条件受限
在服务端渲染中，created和beforeCreate之外的生命周期钩子不可用，因此项目引用的第三方的库也不可用其它生命周期钩子，这对引用库的选择产生了很大的限制；

#### 安全问题

因为做了node服务，因此安全方面也需要考虑DDOS攻击和sql注入

#### 学习成本相对较高
除了对webpack、Vue要熟悉，还需要掌握node、Express相关技术。相对于客户端渲染，项目构建、部署过程更加复杂。


#### 26.`new Vue()` 发生了什么？
1）`new Vue()`是创建`Vue`实例，它内部执行了根实例的初始化过程。

2）具体包括以下操作：
* 选项合并
* `$children`，`$refs`，`$slots`，`$createElement`等实例属性的方法初始化
* 自定义事件处理
* 数据响应式处理
* 生命周期钩子调用 （`beforecreate created`）
* 可能的挂载

### 总结：

`new Vue()`创建了根实例并准备好数据和方法，未来执行挂载时，此过程还会递归的应用于它的子组件上，最终形成一个有紧密关系的组件实例树。

#### 27.`Vue.use`是干什么的？原理是什么？
>`vue.use` 是用来使用插件的，我们可以在插件中扩展全局组件、指令、原型方法等。

1､检查插件是否注册，若已注册，则直接跳出；

2､处理入参，将第一个参数之后的参数归集，并在首部塞入 `this` 上下文；

3､执行注册方法，调用定义好的 `install` 方法，传入处理的参数，若没有 `install` 方法并且插件本身为 `function` 则直接进行注册；

1) 插件不能重复的加载

`install` 方法的第一个参数是vue的构造函数，其他参数是Vue.set中除了第一个参数的其他参数； 代码：`args.unshift(this)`

2) 调用插件的install 方法 代码：
```
typeof plugin.install === 'function'
```

3) 插件本身是一个函数，直接让函数执行。 代码：
```
plugin.apply(null, args)
```

4) 缓存插件。  代码：
```
installedPlugins.push(plugin)
```

#### 28.请说一下响应式数据的理解？

1) 对象内部通过`defineReactive`方法，使用 `Object.defineProperty()` 监听数据属性的 `get` 来进行数据依赖收集，再通过 `set` 来完成数据更新的派发；

2) 数组则通过重写数组方法来实现的。扩展它的 7 个变更⽅法，通过监听这些方法可以做到依赖收集和派发更新；

#### 对应源码
```javascript
Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter () {
      const value = getter ? getter.call(obj) : val
      if (Dep.target) {
        dep.depend() // ** 收集依赖 ** /
        if (childOb) {
          childOb.dep.depend()
          if (Array.isArray(value)) {
            dependArray(value)
          }
        }
      }
      return value
    },
    set: function reactiveSetter (newVal) {
      const value = getter ? getter.call(obj) : val
      if (newVal === value || (newVal !== newVal && value !== value)) {
        return
      }
      if (process.env.NODE_ENV !== 'production' && customSetter) {
        customSetter()
      }
      val = newVal
      childOb = !shallow && observe(newVal)
      dep.notify() /**通知相关依赖进行更新**/
    }
  })
  ```


#### 29.`Vue`中是如何检测数组变化?

数组考虑性能原因没有用`defineProperty`对数组的每一项进行拦截，而是选择重写数组 方法以进行重写。

当数组调用到这 7 个方法的时候，执行 `ob.dep.notify()` 进行派发通知 `Watcher` 更新；

在Vue中修改数组的索引和长度是无法监控到的。

需要通过以下7种变异方法修改数组才会触发数组对应的`wacther`进行更新。

数组中如果是对象数据类型也会进行递归劫持。

#### 说明：那如果想要改索引更新数据怎么办？

可以通过`Vue.set()`来进行处理 =》 核心内部用的是 `splice` 方法。


#### 源码
```javascript
const arrayProto = Array.prototype
export const arrayMethods = Object.create(arrayProto)
const methodsToPatch = [
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse'
]
methodsToPatch.forEach(function (method) { // 重写原型方法
  const original = arrayProto[method] // 调用原数组的方法
  def(arrayMethods, method, function mutator (...args) {
    const result = original.apply(this, args)
    const ob = this.__ob__
    let inserted
    switch (method) {
      case 'push':
      case 'unshift':
        inserted = args
        break
      case 'splice':
        inserted = args.slice(2)
        break
    }
    if (inserted) ob.observeArray(inserted)
    // notify change
    ob.dep.notify() // 当调用数组方法后，手动通知视图更新
    return result
  })
})
this.observeArray(value) // 进行深度监控
```

#### 30.`Vue.set` 方法是如何实现的？

### 核心答案
为什么`$set`可以触发更新，我们给对象和数组本身都增加了`dep`属性，当给对象新增不存在的属性则触发对象依赖的`watcher`去更新，当修改数组索引时我们调用数组本身的`splice`方法去更新数组。

### 补充答案
1) 如果是数组，调用重写的splice方法 （这样可以更新视图 ）
代码：
```
target.splice(key, 1, val)
```
2) 如果不是响应式的也不需要将其定义成响应式属性。
3) 如果是对象，将属性定义成响应式的  
```
defineReactive(ob.value, key, val)
```

### 源码地址
```javascript
export function set (target: Array<any> | Object, key: any, val: any): any {
  if (process.env.NODE_ENV !== 'production' &&
    (isUndef(target) || isPrimitive(target))
  ) {
    warn(`Cannot set reactive property on undefined, null, or primitive value: ${(target: any)}`)
  }
  if (Array.isArray(target) && isValidArrayIndex(key)) {
    target.length = Math.max(target.length, key)
    target.splice(key, 1, val)
    return val
  }
  if (key in target && !(key in Object.prototype)) {
    target[key] = val
    return val
  }
  const ob = (target: any).__ob__
  if (target._isVue || (ob && ob.vmCount)) {
    process.env.NODE_ENV !== 'production' && warn(
      'Avoid adding reactive properties to a Vue instance or its root $data ' +
      'at runtime - declare it upfront in the data option.'
    )
    return val
  }
  if (!ob) {
    target[key] = val
    return val
  }
  defineReactive(ob.value, key, val)
  ob.dep.notify()
  return val
}
```

#### 31.`Vue`中模板编译原理?
### 核心答案
将`template`转换成`render`函数
### 补充说明
这里要注意的是我们在开发时尽量不要使用template.

因为将template转化成render方法需要在运行时进行编译操作会有性能损耗，同时引用带有complier包的vue体积也会变大.

默认.vue文件中的 template处理是通过vue-loader 来进行处理的并不是通过运行时的编译。
### 流程如下
1) 将 template 模板转换成 ast 语法树 - parserHTML

2) 对静态语法做静态标记 - markUp

3) 重新生成代码 - codeGen

### 源码地址
```javascript
function baseCompile (
  template: string,
  options: CompilerOptions
) {
  const ast = parse(template.trim(), options) // 1.将模板转化成ast语法树
  if (options.optimize !== false) {           // 2.优化树
    optimize(ast, options)
  }
  const code = generate(ast, options)         // 3.生成树
  return {
    ast,
    render: code.render,
    staticRenderFns: code.staticRenderFns
  }
})
const ncname = `[a-zA-Z_][\\-\\.0-9_a-zA-Z]*`; 
const qnameCapture = `((?:${ncname}\\:)?${ncname})`;
const startTagOpen = new RegExp(`^<${qnameCapture}`); // 标签开头的正则 捕获的内容是标签名
const endTag = new RegExp(`^<\\/${qnameCapture}[^>]*>`); // 匹配标签结尾的  </div>
const attribute = /^\s*([^\s"'<>\/=]+)(?:\s*(=)\s*(?:"([^"]*)"+|'([^']*)'+|([^\s"'=<>`]+)))?/; // 匹配属性的
const startTagClose = /^\s*(\/?)>/; // 匹配标签结束的  >
let root;
let currentParent;
let stack = []
function createASTElement(tagName,attrs){
    return {
        tag:tagName,
        type:1,
        children:[],
        attrs,
        parent:null
    }
}
function start(tagName,attrs){
    let element = createASTElement(tagName,attrs);
    if(!root){
        root = element;
    }
    currentParent = element;
    stack.push(element);
}
function chars(text){
    currentParent.children.push({
        type:3,
        text
    })
}
function end(tagName){
    const element = stack[stack.length-1];
    stack.length --; 
    currentParent = stack[stack.length-1];
    if(currentParent){
        element.parent = currentParent;
        currentParent.children.push(element)
    }
}
function parseHTML(html){
    while(html){
        let textEnd = html.indexOf('<');
        if(textEnd == 0){
            const startTagMatch = parseStartTag();
            if(startTagMatch){
                start(startTagMatch.tagName,startTagMatch.attrs);
                continue;
            }
            const endTagMatch = html.match(endTag);
            if(endTagMatch){
                advance(endTagMatch[0].length);
                end(endTagMatch[1])
            }
        }
        let text;
        if(textEnd >=0 ){
            text = html.substring(0,textEnd)
        }
        if(text){
            advance(text.length);
            chars(text);
        }
    }
    function advance(n) {
        html = html.substring(n);
    }
    function parseStartTag(){
        const start = html.match(startTagOpen);
        if(start){
            const match = {
                tagName:start[1],
                attrs:[]
            }
            advance(start[0].length);
            let attr,end
            while(!(end = html.match(startTagClose)) && (attr=html.match(attribute))){
                advance(attr[0].length);
                match.attrs.push({name:attr[1],value:attr[3]})
            }
            if(end){
                advance(end[0].length);
                return match
            }
        }
    }
}
// 生成语法树
parseHTML(`<div id="container"><p>hello<span>zf</span></p></div>`);
function gen(node){
    if(node.type == 1){
        return generate(node);
    }else{
        return `_v(${JSON.stringify(node.text)})`
    }
}
function genChildren(el){
    const children = el.children;
    if(el.children){
        return `[${children.map(c=>gen(c)).join(',')}]`
    }else{
        return false;
    }
}
function genProps(attrs){
    let str = '';
    for(let i = 0; i < attrs.length;i++){
        let attr = attrs[i];
        str+= `${attr.name}:${attr.value},`;
    }
    return `{attrs:{${str.slice(0,-1)}}}`
}
function generate(el){
    let children = genChildren(el);
    let code = `_c('${el.tag}'${
        el.attrs.length? `,${genProps(el.attrs)}`:''
    }${
        children? `,${children}`:''
    })`;
    return code;
}
// 根据语法树生成新的代码
let code = generate(root);
let render = `with(this){return ${code}}`;
// 包装成函数
let renderFn = new Function(render);
console.log(renderFn.toString());
```


#### 32.`Proxy` 与 `Object.defineProperty` 优劣对比?
### Proxy 的优势如下:
1）`Proxy` 可以直接监听对象而非属性；

2）`Proxy` 可以直接监听数组的变化；

3）`Proxy` 有多达 13 种拦截方法,不限于 apply、ownKeys、deleteProperty、has 等等是 Object.defineProperty 不具备的；

4）`Proxy` 返回的是一个新对象,我们可以只操作新的对象达到目的,而 Object.defineProperty 只能遍历对象属性直接修改；

5）Proxy 作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利；

### Object.defineProperty 的优势如下:
兼容性好，支持 IE9，而 Proxy 的存在浏览器兼容性问题，而且无法用 polyfill 磨平，因此 Vue 的作者才声明需要等到下个大版本( 3.0 )才能用 Proxy 重写。


#### 33.`Vue3.x`响应式数据原理?

`Vue3.x`改用`Proxy`替代`Object.defineProperty`。

因为`Proxy`可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

`Proxy`只会代理对象的第一层，那么`Vue3`又是怎样处理这个问题的呢？
判断当前`Reflect.get`的返回值是否为`Object`，如果是则再通过`reactive`方法做代理， 这样就实现了深度观测。

监测数组的时候可能触发多次`get/set`，那么如何防止触发多次呢？
我们可以判断`key`是否为当前被代理对象`target`自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行`trigger`。

#### 34.`Vue`的生命周期方法有哪些？
>总共分为8个阶段：创建前/后，载入前/后，更新前/后，销毁前/后。

### 1､创建前/后：
1) `beforeCreate`阶段：`vue`实例的挂载元素`el`和数据对象`data`都为`undefined`，还未初始化。

说明：在当前阶段data、methods、computed以及watch上的数据和方法都不能被访问。

2) created阶段：vue实例的数据对象data有了，el还没有。

说明：可以做一些初始数据的获取，在当前阶段无法与Dom进行交互，如果非要想，可以通过vm.$nextTick来访问Dom。

### 2､载入前/后：
1) beforeMount阶段：vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点。
说明：当前阶段虚拟Dom已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发updated。

2) mounted阶段：vue实例挂载完成，data.message成功渲染。

说明：在当前阶段，真实的Dom挂载完毕，数据完成双向绑定，可以访问到Dom节点，使用$refs属性对Dom进行操作。

## 3､更新前/后：
1) beforeUpdate阶段：响应式数据更新时调用，发生在虚拟DOM打补丁之前，适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。

说明：可以在当前阶段进行更改数据，不会造成重渲染。

2) updated阶段：虚拟DOM重新渲染和打补丁之后调用，组成新的DOM已经更新，避免在这个钩子函数中操作数据，防止死循环。

说明：当前阶段组件Dom已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

## 4､销毁前/后：
1) beforeDestroy阶段：实例销毁前调用，实例还可以用，this能获取到实例，常用于销毁定时器，解绑事件。

说明：在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

2) destroyed阶段：实例销毁后调用，调用后所有事件监听器会被移除，所有的子实例都会被销毁。

说明：当前阶段组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。


### 补充说明
第一次页面加载时会触发：beforeCreate, created, beforeMount, mounted。

1) created 实例已经创建完成，因为它是最早触发的原因可以进行一些数据，资源的请求。(服务器渲染支持created方法)

2) mounted 实例已经挂载完成，可以进行一些DOM操作。(接口请求)

### 生命周期图
![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2011101150_1604980206089.png)


#### 源码
```js
/* @flow */

import config from '../config'
import Watcher from '../observer/watcher'
import { mark, measure } from '../util/perf'
import { createEmptyVNode } from '../vdom/vnode'
import { updateComponentListeners } from './events'
import { resolveSlots } from './render-helpers/resolve-slots'
import { toggleObserving } from '../observer/index'
import { pushTarget, popTarget } from '../observer/dep'

import {
  warn,
  noop,
  remove,
  emptyObject,
  validateProp,
  invokeWithErrorHandling
} from '../util/index'

export let activeInstance: any = null
export let isUpdatingChildComponent: boolean = false

export function setActiveInstance(vm: Component) {
  const prevActiveInstance = activeInstance
  activeInstance = vm
  return () => {
    activeInstance = prevActiveInstance
  }
}

export function initLifecycle (vm: Component) {
  const options = vm.$options

  // locate first non-abstract parent
  let parent = options.parent
  if (parent && !options.abstract) {
    while (parent.$options.abstract && parent.$parent) {
      parent = parent.$parent
    }
    parent.$children.push(vm)
  }

  vm.$parent = parent
  vm.$root = parent ? parent.$root : vm

  vm.$children = []
  vm.$refs = {}

  vm._watcher = null
  vm._inactive = null
  vm._directInactive = false
  vm._isMounted = false
  vm._isDestroyed = false
  vm._isBeingDestroyed = false
}

export function lifecycleMixin (Vue: Class<Component>) {
  Vue.prototype._update = function (vnode: VNode, hydrating?: boolean) {
    const vm: Component = this
    const prevEl = vm.$el
    const prevVnode = vm._vnode
    const restoreActiveInstance = setActiveInstance(vm)
    vm._vnode = vnode
    // Vue.prototype.__patch__ is injected in entry points
    // based on the rendering backend used.
    if (!prevVnode) {
      // initial render
      vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */)
    } else {
      // updates
      vm.$el = vm.__patch__(prevVnode, vnode)
    }
    restoreActiveInstance()
    // update __vue__ reference
    if (prevEl) {
      prevEl.__vue__ = null
    }
    if (vm.$el) {
      vm.$el.__vue__ = vm
    }
    // if parent is an HOC, update its $el as well
    if (vm.$vnode && vm.$parent && vm.$vnode === vm.$parent._vnode) {
      vm.$parent.$el = vm.$el
    }
    // updated hook is called by the scheduler to ensure that children are
    // updated in a parent's updated hook.
  }

  Vue.prototype.$forceUpdate = function () {
    const vm: Component = this
    if (vm._watcher) {
      vm._watcher.update()
    }
  }

  Vue.prototype.$destroy = function () {
    const vm: Component = this
    if (vm._isBeingDestroyed) {
      return
    }
    callHook(vm, 'beforeDestroy')
    vm._isBeingDestroyed = true
    // remove self from parent
    const parent = vm.$parent
    if (parent && !parent._isBeingDestroyed && !vm.$options.abstract) {
      remove(parent.$children, vm)
    }
    // teardown watchers
    if (vm._watcher) {
      vm._watcher.teardown()
    }
    let i = vm._watchers.length
    while (i--) {
      vm._watchers[i].teardown()
    }
    // remove reference from data ob
    // frozen object may not have observer.
    if (vm._data.__ob__) {
      vm._data.__ob__.vmCount--
    }
    // call the last hook...
    vm._isDestroyed = true
    // invoke destroy hooks on current rendered tree
    vm.__patch__(vm._vnode, null)
    // fire destroyed hook
    callHook(vm, 'destroyed')
    // turn off all instance listeners.
    vm.$off()
    // remove __vue__ reference
    if (vm.$el) {
      vm.$el.__vue__ = null
    }
    // release circular reference (#6759)
    if (vm.$vnode) {
      vm.$vnode.parent = null
    }
  }
}

export function mountComponent (
  vm: Component,
  el: ?Element,
  hydrating?: boolean
): Component {
  vm.$el = el
  if (!vm.$options.render) {
    vm.$options.render = createEmptyVNode
    if (process.env.NODE_ENV !== 'production') {
      /* istanbul ignore if */
      if ((vm.$options.template && vm.$options.template.charAt(0) !== '#') ||
        vm.$options.el || el) {
        warn(
          'You are using the runtime-only build of Vue where the template ' +
          'compiler is not available. Either pre-compile the templates into ' +
          'render functions, or use the compiler-included build.',
          vm
        )
      } else {
        warn(
          'Failed to mount component: template or render function not defined.',
          vm
        )
      }
    }
  }
  callHook(vm, 'beforeMount')

  let updateComponent
  /* istanbul ignore if */
  if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
    updateComponent = () => {
      const name = vm._name
      const id = vm._uid
      const startTag = `vue-perf-start:${id}`
      const endTag = `vue-perf-end:${id}`

      mark(startTag)
      const vnode = vm._render()
      mark(endTag)
      measure(`vue ${name} render`, startTag, endTag)

      mark(startTag)
      vm._update(vnode, hydrating)
      mark(endTag)
      measure(`vue ${name} patch`, startTag, endTag)
    }
  } else {
    updateComponent = () => {
      vm._update(vm._render(), hydrating)
    }
  }

  // we set this to vm._watcher inside the watcher's constructor
  // since the watcher's initial patch may call $forceUpdate (e.g. inside child
  // component's mounted hook), which relies on vm._watcher being already defined
  new Watcher(vm, updateComponent, noop, {
    before () {
      if (vm._isMounted && !vm._isDestroyed) {
        callHook(vm, 'beforeUpdate')
      }
    }
  }, true /* isRenderWatcher */)
  hydrating = false

  // manually mounted instance, call mounted on self
  // mounted is called for render-created child components in its inserted hook
  if (vm.$vnode == null) {
    vm._isMounted = true
    callHook(vm, 'mounted')
  }
  return vm
}

export function updateChildComponent (
  vm: Component,
  propsData: ?Object,
  listeners: ?Object,
  parentVnode: MountedComponentVNode,
  renderChildren: ?Array<VNode>
) {
  if (process.env.NODE_ENV !== 'production') {
    isUpdatingChildComponent = true
  }

  // determine whether component has slot children
  // we need to do this before overwriting $options._renderChildren.

  // check if there are dynamic scopedSlots (hand-written or compiled but with
  // dynamic slot names). Static scoped slots compiled from template has the
  // "$stable" marker.
  const newScopedSlots = parentVnode.data.scopedSlots
  const oldScopedSlots = vm.$scopedSlots
  const hasDynamicScopedSlot = !!(
    (newScopedSlots && !newScopedSlots.$stable) ||
    (oldScopedSlots !== emptyObject && !oldScopedSlots.$stable) ||
    (newScopedSlots && vm.$scopedSlots.$key !== newScopedSlots.$key)
  )

  // Any static slot children from the parent may have changed during parent's
  // update. Dynamic scoped slots may also have changed. In such cases, a forced
  // update is necessary to ensure correctness.
  const needsForceUpdate = !!(
    renderChildren ||               // has new static slots
    vm.$options._renderChildren ||  // has old static slots
    hasDynamicScopedSlot
  )

  vm.$options._parentVnode = parentVnode
  vm.$vnode = parentVnode // update vm's placeholder node without re-render

  if (vm._vnode) { // update child tree's parent
    vm._vnode.parent = parentVnode
  }
  vm.$options._renderChildren = renderChildren

  // update $attrs and $listeners hash
  // these are also reactive so they may trigger child update if the child
  // used them during render
  vm.$attrs = parentVnode.data.attrs || emptyObject
  vm.$listeners = listeners || emptyObject

  // update props
  if (propsData && vm.$options.props) {
    toggleObserving(false)
    const props = vm._props
    const propKeys = vm.$options._propKeys || []
    for (let i = 0; i < propKeys.length; i++) {
      const key = propKeys[i]
      const propOptions: any = vm.$options.props // wtf flow?
      props[key] = validateProp(key, propOptions, propsData, vm)
    }
    toggleObserving(true)
    // keep a copy of raw propsData
    vm.$options.propsData = propsData
  }

  // update listeners
  listeners = listeners || emptyObject
  const oldListeners = vm.$options._parentListeners
  vm.$options._parentListeners = listeners
  updateComponentListeners(vm, listeners, oldListeners)

  // resolve slots + force update if has children
  if (needsForceUpdate) {
    vm.$slots = resolveSlots(renderChildren, parentVnode.context)
    vm.$forceUpdate()
  }

  if (process.env.NODE_ENV !== 'production') {
    isUpdatingChildComponent = false
  }
}

function isInInactiveTree (vm) {
  while (vm && (vm = vm.$parent)) {
    if (vm._inactive) return true
  }
  return false
}

export function activateChildComponent (vm: Component, direct?: boolean) {
  if (direct) {
    vm._directInactive = false
    if (isInInactiveTree(vm)) {
      return
    }
  } else if (vm._directInactive) {
    return
  }
  if (vm._inactive || vm._inactive === null) {
    vm._inactive = false
    for (let i = 0; i < vm.$children.length; i++) {
      activateChildComponent(vm.$children[i])
    }
    callHook(vm, 'activated')
  }
}

export function deactivateChildComponent (vm: Component, direct?: boolean) {
  if (direct) {
    vm._directInactive = true
    if (isInInactiveTree(vm)) {
      return
    }
  }
  if (!vm._inactive) {
    vm._inactive = true
    for (let i = 0; i < vm.$children.length; i++) {
      deactivateChildComponent(vm.$children[i])
    }
    callHook(vm, 'deactivated')
  }
}

export function callHook (vm: Component, hook: string) {
  // #7573 disable dep collection when invoking lifecycle hooks
  pushTarget()
  const handlers = vm.$options[hook]
  const info = `${hook} hook`
  if (handlers) {
    for (let i = 0, j = handlers.length; i < j; i++) {
      invokeWithErrorHandling(handlers[i], vm, null, vm, info)
    }
  }
  if (vm._hasHookEvent) {
    vm.$emit('hook:' + hook)
  }
  popTarget()
}
```


#### 35.生命周期钩子是如何实现的?
#### 核心答案：
Vue的生命周期钩子就是回调函数而已，当创建组件实例的过程中会调用对应的钩子方法。

#### 补充说明
内部主要是使用callHook方法来调用对应的方法。核心是一个发布订阅模式，将钩子订阅好(内部采用数组的方式存储)，在对应的阶段进行发布。


#### 36.`Vue` 的父组件和子组件生命周期钩子执行顺序?

### 核心答案：
第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子。

### 渲染过程：
父组件挂载完成一定是等子组件都挂载完成后，才算是父组件挂载完，所以父组件的mounted在子组件mouted之后

* 父beforeCreate -> 
* 父created -> 
* 父beforeMount -> 
* 子beforeCreate -> 
* 子created -> 
* 子beforeMount -> 
* 子mounted -> 
* 父mounted

### 子组件更新过程：
影响到父组件：
* 父beforeUpdate -> 
* 子beforeUpdate->
* 子updated -> 
* 父updted

不影响父组件：
* 子beforeUpdate -> 
* 子updated

### 父组件更新过程：
影响到子组件：
* 父beforeUpdate -> 
* 子beforeUpdate->
* 子updated -> 
* 父updted

不影响子组件：
* 父beforeUpdate -> 
* 父updated

### 销毁过程：
* 父beforeDestroy -> 
* 子beforeDestroy -> 
* 子destroyed -> 
* 父destroyed

>重要：父组件等待子组件完成后，才会执行自己对应完成的钩子。


#### 37.组件中写 `name`选项有哪些好处及作用？

### 核心答案：
1) 可以通过名字找到对应的组件 （ 递归组件 ）
2) 可以通过name属性实现缓存功能 (keep-alive)
3) 可以通过name来识别组件 （跨级组件通信时非常重要）

### 源码
```js
Vue.extend = function () {
    if(name) {
        Sub.options.componentd[name] = Sub
    }
}
```


#### 38.`keep-alive`平时在哪里使用？原理是？
### 核心答案：
keep-alive 主要是组件缓存，采用的是LRU算法。最近最久未使用法。

常用的两个属性include/exclude，允许组件有条件的进行缓存。

两个生命周期activated/deactivated，用来得知当前组件是否处于活跃状态。

### 源码
```javascript
abstract: true, // 抽象组件 
props:{
    include: patternTypes,  // 要缓存的有哪些
    exclude: patternTypes, // 要排除的有哪些
    max: [String, Number] //最大缓存数量 
}
if(cache[key]) { // 通过key 找到缓存，获取实例
    vnode.componentInstance = cache[key].componentInstance
    remove(keys, key) //将key删除掉 
    keys.push(key) // 放到末尾 
} else {
    cache[key] = vnode // 没有缓存过 
    keys.push(key) //存储key
    if(this.max && keys.length > parseInt(this.max)) { // 如果超过最大缓存数 
    // 删除最早缓存的 
    pruneCacheEntry(cache, keys[0], keys, this._vnode)
}
}
vnode.data.keepAlive = true // 标记走了缓存 
```

#### 39.`Vue.minxin`的使用场景和原理？
### 核心答案：
Vue.mixin的作用就是抽离公共的业务逻辑，原理类似“对象的继承”，当组件初始化时会调用 mergeOptions方法进行合并，采用策略模式针对不同的属性进行合并，如果混入的数据和本身组件中的数据冲突，会采用“就近原则”以组件的数据为准。
### 补充回答：
`mixin`中有很多缺陷“命名冲突问题”，“依赖问题”，“数据来源问题”，这里强调一下`mixin`的数据是不会被共享的。
### 注意事项
如果只是提取公用的数据或者通用的方法，并且这些数据或者方法，不需要组件间进行维护，就可以使用`mixins`。（类似于js中封装的一些公用的方法）


#### 40.`mixins`和`vuex`的区别?
`vuex`公共状态管理，在一个组件被引入后，如果该组件改变了vuex里面的数据状态，其他引入vuex数据的组件也会对应修改，所有的vue组件应用的都是同一份vuex数据。（在js中，有点类似于浅拷贝）

`vue`引入`mixins`数据，`mixins`数据或方法，在每一个组件中都是独立的，互不干扰的，都属于vue组件自身。（在js中，有点类似于深度拷贝）



#### 41.`mixins`和公共组件的区别?
通用的数据和方法，确实可以提出一个通用的组件，由父子组件传参的形式进行分享公用。
### 公共组件
子组件通过props接收来自父组件（公共组件）的参数或者方法，但vue不建议，子组件直接修改props接收到的父组件的数据。需要在子组件的data中或者computed中定义一个字段来接收。（有点麻烦）
公共组件最主要的作用还是复用相同的vue组件（有视图，有方法，有状态
)。
### mixins
如果只是提取公用的数据或者通用的方法，并且这些数据或者方法，不需要组件间进行维护，就可以使用mixins。（类似于js中封装的一些公用的方法）


#### 42.`Vue-router`有几种钩子函数？具体是什么及执行流程是怎样的？
### 核心答案：
路由钩子的执行流程，钩子函数种类有：全局守卫、路由守卫、组件守卫。

### 完整的导航解析流程
`1.`导航被触发；

`2.`在失活的组件里调用beforeRouteLeave守卫；

`3.`调用全局beforeEach守卫；

`4.`在复用组件里调用beforeRouteUpdate守卫；

`5.`调用路由配置里的beforeEnter守卫；

`6.`解析异步路由组件；

`7.`在被激活的组件里调用beforeRouteEnter守卫；

`8.`调用全局beforeResolve守卫；

`9.`导航被确认；

`10.`调用全局的afterEach钩子；

`11.`DOM更新；

`12.`用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。




#### 43.`vue-router` 两种模式的区别？

### 核心答案：
`vue-router` 有 3 种路由模式：`hash`、`history`、`abstract`。

1) `hash`模式：hash + hashChange

特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。通过监听 hash（#）的变化来执行js代码 从而实现 页面的改变。 

### 核心代码：
```js
window.addEventListener(‘hashchange‘,function(){
    self.urlChange()
})
```
2) `history`模式：historyApi + popState

`HTML5`推出的history API，由pushState()记录操作历史，监听popstate事件来监听到状态变更；
因为 只要刷新 这个url（www.ff.ff/jjkj/fdfd/fdf/fd）就会请求服务器，然而服务器上根本没有这个资源，所以就会报404，解决方案就 配置一下服务器端。
#### 说明：

1）`hash`: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器；

2）`history` : 依赖 HTML5 History API 和服务器配置。具体可以查看 HTML5 History 模式；

3）`abstract` : 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式.


#### 44.`Vue` 为什么需要虚拟DOM？  虚拟`DOM`的优劣如何？

## 核心答案：
`Virtual DOM` 就是用js对象来描述真实DOM，是对真实DOM的抽象，由于直接操作DOM性能低但是js层的操作效率高，可以将DOM操作转化成对象操作，最终通过diff算法比对差异进行更新DOM (减少了对真实DOM的操作)。虚拟DOM不依赖真实平台环境从而也可以实现跨平台。

## 补充回答：
`虚拟DOM`的实现就是普通对象包含tag、data、hildren等属性对真实节点的描述。（本质上就是在JS和DOM之间的一个缓存）

`Vue2`的 Virtual DOM 借鉴了开源库snabbdom的实现。

`VirtualDOM`映射到真实DOM要经历VNode的create、diff、patch等阶段。


#### 45.`Vue`中`key`的作用和工作原理，说说你对它的理解

### 核心答案：
#### 例如：
```js
v-for="(item, index) in tableList" :key="index"
```

key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作量，提高性能。

#### 补充回答：
1) 若不设置key还可能在列表更新时引发一些隐蔽的bug

2) vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。



#### 46.说说`$nextTick`的使用?

1) `nextTick`的回调是在下次`DOM`更新循环结束之后执行的延迟回调。

2)  在修改数据之后立即使用这个方法，获取更新后的`DOM`。

3) `nextTick`主要使用了宏任务和微任务。

#### 47.`computed` 和 `watch` 的区别和运用的场景？

### 核心答案：
`computed`： 

计算属性。依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值；

`watch`： 

监听数据的变化。更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；

### 运用场景：
1）当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算；

2）当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。

这些都是计算属性无法做到的。

#### 48.如何理解自定义指令？

#### 核心答案：
>指令的实现原理，可以从编译原理 =>代码生成=> 指令钩子实现进行概述

1）在生成 ast 语法树时，遇到指令会给当前元素添加directives属性

2）通过 genDirectives 生成指令代码

3）在patch前将指令的钩子提取到 cbs中，在patch过程中调用对应的钩子。

4）当执行指令对应钩子函数时，调用对应指令定义的方法


#### 49.`$router`和`$route`的区别？

`this.$router`与`this.$route` 两者相差 一个 `r` 但是差距蛮大的。

`Vue Router`是`Vue.js`的路由管理器，在Vue实例内部，可以通过`$router`访问路由实例

通过`$route`可以访问当前激活的路由的状态信息，包含了当前`URL`解析得到的信息，还有URL匹配到的路由记录，可以将`$router`理解为一个容器去管理了一组`$route`，

而`$route`是进行了当前URL和组件的映射。


#### 50.`$router`对象有哪些属性？
* `$router.app`: 配置了router的Vue根实例。

* `$router.mode`: 路由使用的模式。

* `$router.currentRoute`: 当前路由对应的路由信息对象。

#### 51.`$router`对象有哪些方法？
* $router.beforeEach(to, from, next)
* $router.beforeResolve(to, from, next)
* $router.afterEach(to, from)
* $router.push(location[, onComplete[, onAbort]])
* $router.replace(location[, onComplete[, onAbort]])
* $router.go(n)
* $router.back()$router.back
* $history.forward()
* $router.getMatchedComponents([location])
* $router.resolve(location[, current[, append]])
* $router.addRoutes(route)
* $router.onReady(callback[, errorCallback])
* $router.onError(callback)

### 52.`$route`对象有哪些属性？
* `$route.path`: 
返回字符串，对应当前路由的路径，总是解析为绝对路径。

* `$route.params`: 
返回一个key-value对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。

* `$route.query`:
 返回一个key-value对象，表示URL查询参数。

* `$route.hash`: 
返回当前路由的带#的hash值，如果没有hash值，则为空字符串。

* `$route.fullPath`: 
返回完成解析后的URL，包含查询参数和hash的完整路径。

* `$route.matched`: 
返回一个数组，包含当前路由的所有嵌套路径片段的路由记录，路由记录就是routes配置数组中的对象副本。

* `$route.name`: 
如果存在当前路由名称则返回当前路由的名称。

* `$route.redirectedFrom`: 
如果存在重定向，即为重定向来源的路由的名字。


#### 53.对`MVC （react）` `MVVM（vue）`的了解?
### 标签  腾讯  阿里  西门子

### 什么是MVC
* `M（modal）`：是应用程序中处理数据逻辑的部分。
* `V （view）`  ：是应用程序中数据显示的部分。
* `C（controller）`：是应用程序中处理用户交互的地方

### 什么是MVVM
* M（modal）：模型，定义数据结构。
* C（controller）：实现业务逻辑，数据的增删改查。在MVVM模式中一般把C层算在M层中，（只有在理想的双向绑定模式下，Controller 才会完全的消失。这种理想状态一般不存在）。
* VM（viewModal）：视图View的模型、映射和显示逻辑（如if for等，非业务逻辑），另外绑定器也在此层。ViewModel是基于视图开发的一套模型，如果你的应用是给盲人用的，那么也可以开发一套基于Audio的模型AudioModel。
* V（view） ：将ViewModel通过特定的GUI展示出来，并在GUI控件上绑定视图交互事件，V(iew)一般由MVVM框架自动生成在浏览器中。

### React与Vue对比

### 相似点：
1. 使用 Virtual DOM
2. 提供了响应式 (Reactive) 和组件化 (Composable) 的视图组件。
3. 将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库。

### 区别：
1. 在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。如要避免不必要的子组件的重渲染，你需要手动实现；在 Vue 应用中，组件的依赖是在渲染过程中自动追踪的，所以系统能精确知晓哪个组件确实需要被重渲染，开发者不需要考虑组件是否需要重新渲染之类的优化。

2. 在React中，一切都是JavaScript，所有的组件的渲染功能都依靠 JSX。JSX 是使用 XML 语法编写 JavaScript 的一种语法糖。你可以使用完整的编程语言 JavaScript 功能来构建你的视图页面；在Vue中有自带的渲染函数，Vue也支持JSX，Vue官方推荐使用模板渲染视图。组件分为逻辑类组件和表现类组件。

3. 组件作用域内的CSS。CSS 作用域在 React 中是通过 CSS-in-JS 的方案实现的；在Vue中是通过给style标签加scoped标记实现的。

4. Vue 的路由库和状态管理库都是由官方维护支持且与核心库同步更新的。React 则是选择把这些问题交给社区维护，因此创建了一个更分散的生态系统。


### 54.`Vue.js` 如何让`CSS`只在当前组件中起作⽤?

将当前组件的 `<style>` 修改为 `<style scoped>`

#### 55.`Vue` 中的`diff`原理?

### 核心答案：
vue的diff算法是平级比较，不考虑跨级比较的情况。内部采用深度递归的方式 + 双指针的方式进行比较。

### 补充回答：
1) 先比较是否是相同节点

2) 相同节点比较属性，并复用老节点

3) 比较儿子节点，考虑老节点和新节点儿子的情况

4) 优化比较：头头、尾尾、头尾、尾头

5) 比对查找进行复用

### Vue2 与 Vue3.x 的diff算法：
Vue2的核心Diff算法采用了双端比较的算法，同时从新旧children的两端开始进行比较，借助key值找到可复用的节点，再进行相关操作。

Vue3.x借鉴了ivi算法和 inferno算法，该算法中还运用了动态规划的思想求解最长递归子序列。(实际的实现可以结合Vue3.x源码看。)

### 56.Vuex的5个核心属性是什么？
分别是 state、getters、mutations、actions、modules 。

### 57.在Vuex中使用mutation要注意什么?
mutation 必须是同步函数

### 58.Vuex中action和mutation有什么相同点？
第二参数都可以接收外部提交时传来的参数。 
```
this.$store.dispatch('ACTION_NAME',data)
和
this.$store.commit('SET_NUMBER',10)
```
在组件中多次提交同一个action，怎么写使用更方便。

### 59.那为什么new Vue里data可以是一个对象？
```js
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: {App}
})
```
>原因：因为JS里的对象是引用关系，而且new Vue是不会被复用的，所以不存在引用对象的问题。

### 60.Vue 3.0 性能提升主要是通过哪几方面体现的？

### 1.响应式系统提升
vue2在初始化的时候，对data中的每个属性使用definepropery调用getter和setter使之变为响应式对象。如果属性值为对象，还会递归调用defineproperty使之变为响应式对象。

vue3使用proxy对象重写响应式。proxy的性能本来比defineproperty好，proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性，另外有多层属性嵌套的话，只有访问某个属性的时候，才会递归处理下一级的属性。

### 优势：
* 可以监听动态新增的属性；
* 可以监听删除的属性 ；
* 可以监听数组的索引和 length 属性；
### 2. 编译优化
优化编译和重写虚拟dom，让首次渲染和更新dom性能有更大的提升 vue2 通过标记静态根节点,优化 diff 算法 vue3 标记和提升所有静态根节点,diff 的时候只比较动态节点内容

Fragments, 模板里面不用创建唯一根节点,可以直接放同级标签和文本内容

### 静态提升
patch flag, 跳过静态节点,直接对比动态节点,缓存事件处理函数

### 3. 源码体积的优化
vue3移除了一些不常用的api，例如：inline-template、filter等 使用tree-shaking


### 61.Composition Api 与 Vue 2.x使用的Options Api 有什么区别？

### Options Api
包含一个描述组件选项（data、methods、props等）的对象 options；

API开发复杂组件，同一个功能逻辑的代码被拆分到不同选项 ；

使用mixin重用公用代码，也有问题：命名冲突，数据来源不清晰；

### composition Api
vue3 新增的一组 api，它是基于函数的 api，可以更灵活的组织组件的逻辑。

解决options api在大型项目中，options api不好拆分和重用的问题。

### 62.Proxy 相对于 Object.defineProperty区别？

有哪些优点？

proxy的性能本来比defineproperty好，proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性，另外有多层属性嵌套的话，只有访问某个属性的时候，才会递归处理下一级的属性。

* 可以* 监听数组变化
* 可以劫持整个对象
* 操作时不是对原对象操作,是 new Proxy 返回的一个新对象
* 可以劫持的操作有 13 种


### 63.Vue 3.0 在编译方面有哪些优化？

vue.js 3.x中标记和提升所有的静态节点，diff的时候只需要对比动态节点内容；

Fragments（升级vetur插件):
template中不需要唯一根节点，可以直接放文本或者同级标签

静态提升(hoistStatic),当使用 hoistStatic 时,所有静态的节点都被提升到 render 方法之外.只会在应用启动的时候被创建一次,之后使用只需要应用提取的静态节点，随着每次的渲染被不停的复用。

patch flag, 在动态标签末尾加上相应的标记,只能带 patchFlag 的节点才被认为是动态的元素,会被追踪属性的修改,能快速的找到动态节点,而不用逐个逐层遍历，提高了虚拟dom diff的性能。

缓存事件处理函数cacheHandler,避免每次触发都要重新生成全新的function去更新之前的函数 tree shaking 通过摇树优化核心库体积,减少不必要的代码量


### 64.Vue.js 3.0 响应式系统的实现原理？

### 1. reactive
设置对象为响应式对象。接收一个参数，判断这参数是否是对象。不是对象则直接返回这个参数，不做响应式处理。创建拦截器handerler，设置get/set/deleteproperty。

#### get
收集依赖（track）；
如果当前 key 的值是对象，则为当前 key 的对
象创建拦截器 handler, 设置 get/set/deleteProperty；
如果当前的 key 的值不是对象，则返回当前 key 的值。

#### set
设置的新值和老值不相等时，更新为新值，并触发更新（trigger）。

deleteProperty 当前对象有这个 key 的时候，删除这个 key 并触发更新（trigger）。

### 2. effect
接收一个函数作为参数。作用是：访问响应式对象属性时去收集依赖

### 3. track
接收两个参数：target 和 key
－如果没有 activeEffect，则说明没有创建 effect 依赖

－如果有 activeEffect，则去判断 WeakMap 集合中是否有 target 属性

－WeakMap 集合中没有 target 属性，则 set(target, (depsMap = new Map()))

－WeakMap 集合中有 target 属性，则判断 target 属性的 map 值的 depsMap 中是否有 key 属性

－depsMap 中没有 key 属性，则 set(key, (dep = new Set())) －depsMap 中有 key 属性，则添加这个 activeEffect

### 4. trigger

判断 WeakMap 中是否有 target 属性，WeakMap 中有 target 属性，则判断 target 属性的 map 值中是否有 key 属性，有的话循环触发收集的 effect()。