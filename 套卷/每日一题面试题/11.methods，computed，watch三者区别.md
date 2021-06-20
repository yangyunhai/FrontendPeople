### watch
* 当数据改变时，直接触发对应操作；
* 可以监听的数据有三部分，即props，data，computed；
* 所触发的对应操作函数中包含两个参数，第一个参数是新值newValue，第二个参数是旧值oldValue；
* 不支持缓存，但支持异步，在数据变化时执行异步或开销较大的操作时使用；
* 一对多，即一个数据改变时，可以通过触发对应操作改变多个其他的数据。
### computed
* computed 依赖于 data 中的数据，只要在它的相关依赖发生改变时就会触发computed；
* 计算属性是基于属性的依赖进行缓存的，当data中的数据未变时，优先取缓存中的东西；
* 支持缓存，但不支持异步；
* 多对一或一对一，即一个属性是由其他属性计算而来的，这个属性可能是依赖其他一个或多个属性。
### methods
* 是一个方法，执行的时候需要事件进行触发；
* 可以在模板或者js中以方法的形式调用
* 执行次数跟调用次数想对应

### 使用说明
```js
export default {
    data: {
	  userName: '鬼鬼'
	},
    watch:{
      userName(newValue,oldValue){
        console.log(`原来的值：${newValue}`);
        console.log(`新的值${oldValue}`);
      },
      //深度监听   
      userName:{
        handler:function(newValue,oldValue){
           console.log(`原来的值：${newValue}`);
           console.log(`新的值${oldValue}`);
        },
        immediate:true,
        deep:true
      },
    },
	computed: {
	  gUserName() {
	  	return this.userName 
	  },
     //computed传参
      gUserName(keyName){
        return function(keyName){
            return keyName+this.userName
        }
      }
	},
	methods: {
	  getUserName() {
	  	return this.userName
	  }
	}
}
```

```html
<template>
  <!-- methods -->
  <div> 我的名字叫：{{ getUserName() }} </div>
  <!-- computed -->
  <div> 我的名字叫：{{ gUserName }} </div>
  <!-- computed 传参 -->
  <div> {{ gUserName("我的名字叫：") }} </div>
</template>
```
