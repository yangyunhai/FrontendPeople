>这道题在Vue面试岗位还是问的挺多的

一、使用Vuex的目的

实现多组件状态管理，多个组件之间需要数据共享

二、Vuex 的五大核心
>其中state和mutation是必须的，其他可根据需求来加

* 1、state
负责状态管理，类似于vue中的data，用于初始化数据

* 2、mutation
专用于修改state中的数据，通过commit触发

* 3、action
可以处理异步，通过dispatch触发，不能直接修改state，首先在组件中通过dispatch触发action，然后在action函数内部commit触发mutation，通过mutation修改state状态值

* 4、getter
Vuex中的计算属性，相当于vue中的computed,依赖于state状态值，状态值一旦改变，getter会重新计算，也就是说，当一个数据依赖于另一个数据发生变化时，就要使用getter

* 5、module
模块化管理


### 使用流程
* 1.创建store实例并且导出
store.js
```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
    //声明state
    state: { 
      userInfo:{ userName:"" }
    },       
    //声明mutations
    mutations: {
      setUserInfo(state,userInfo){  
        state.userInfo = userInfo
      }
    },   
    //声明actions
    actions: {
      setUserInfo({ commit },userInfo){
        commit('setUserInfo',userInfo)
      }
    },    
    //声明getters
    getters:{
      userName(state){
        return "姓名："+state.userInfo.userName
      }
    }
})
export default store
```

2.引入Vuex
```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'
new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```

3.组件内使用

#### 使用方式一
```html
<template>
 <div>
     <!-- state使用 -->
     <p>{{$store.state.userInfo.userName}}</p>
     <!-- getters使用 -->
     <p>{{$store.getters.userName}}</p>
     <!-- action使用 -->
     <p @click="setInfo">存储信息</p>
 </div>
</template>
```

```js
export default {
   methods: {
      setInfo(){
        this.$store.commit('setUserInfo',{
          userName:"鬼鬼" 
       }) 
     }
   }
}
```


#### 使用方式二
```html
<template>
 <div>
     <!-- state使用 -->
     <p>{{userInfo.userName}}</p>
     <!-- getters使用 -->
     <p>{{userName}}</p>
     <!-- action使用 -->
     <p @click="setInfo">存储信息</p>
 </div>
</template>
```

```js
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
export default {
  methods: {
    ...mapActions(['setUserInfo']),
    // ...mapMutations(["setUserInfo"]),
    setInfo(){
      this.setUserInfo({
         userName:"鬼鬼" 
     })
  },
  computed: {
    ...mapState({ 
        userInfo: state => state.userInfo
     }),
    ...mapGetters(['userName']),
  }
}
```


### 参考资料
* https://blog.csdn.net/u012967771/article/details/114132555
* https://cnblogs.com/leslie1943/p/13370639.html?utm_source=tuicool