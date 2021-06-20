## 面试题专栏说明
>最近刚好换了工作，把最近面试，被面的问题，以及现在大多数出现的面试题以一个专题的形式一次性总结，一天一题，现在总共汇总了`150道题`，感觉如果这些题都能答出来`25k`稳如老狗了，每道题目答案没有多余扯皮的部分，就是单纯的答案。

## 谈谈Vue和React的区别？
### 1.数据流的不同
* 组件与DOM之间可以通过 v-model 双向绑定（双向数据流）
* React单向数据响应，需要手动setState
### 2.监听数据变化的实现原理不同
* Vue通过 getter/setter
* React默认是通过比较引用的方式（diff）进行的
### 3.代码的复用
* Vue是才有公共组件或者mixin的方式实现代码的复用
* React是通过 HoC (高阶组件）实现代码的复用
### 4.组件通信的区别
* Vue是通过props、$emit/$on、$parent/$children、EventBus、vuex、$root等方式实现组件通信
* React可以通过props向子组件传递数据或者回调或者context实现组件通信
### 5.模板渲染方式的不同
> Vue是通过一种拓展的HTML语法进行渲染（其实Vue也是可以使用JSX）
Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现
* React 是通过JSX渲染模板
> React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的
### 6.diff算法不同
* vue比对节点，当节点元素类型相同，但是className不同，认为是不同类型元素，删除重建，而react会认为是同类型节点，只是修改节点属性

* vue的列表比对，采用从两端到中间的比对方式，而react则采用从左到右依次比对的方式。当一个集合，只是把最后一个节点移动到了第一个，react会把前面的节点依次移动，而vue只会把最后一个节点移动到第一个。总体上，vue的对比方式更高效。
### 7.Vuex 和 Redux 的区别
* Vuex 更加灵活一些，组件中既可以 dispatch action 也可以 commit updates，而 Redux 中只能进行 dispatch，并不能直接调用 reducer 进行修改
* Redux 使用的是不可变数据，而Vuex的数据是可变的。Redux每次都是用新的state替换旧的state，而Vuex是直接修改
* Redux 在检测数据变化的时候，是通过 diff 的方式比较差异的，而Vuex其实和Vue的原理一样，是通过 getter/setter来比较的
### 8.数据声明和使用的方式不同
* vue中的数据由data属性在Vue对象中进行管理，react中的数据由state属性管理；
* vue通过slot插槽进行嵌套传递，react通过“props.children”的方式将标签内的部分传递给子组件。

### 说明
>每天一到面试题，25k+稳如老狗，点击↓关注【鬼哥】我
 当前进度【#001题】


### 参考资料
* https://cnblogs.com/mengff/p/12828825.html
* https://zhuanlan.zhihu.com/p/43494278
* https://m.php.cn/article/464234.html
* https://segmentfault.com/a/1190000019208626