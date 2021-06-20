### new实现原理
* 1、创建一个空对象 obj
* 2、将该对象 obj 的原型链 __proto__ 指向构造函数的原型 prototype，
* 并且在原型链 __proto__ 上设置 构造函数 constructor 为要实例化的 Fn
* 3、传入参数，并让 构造函数 Fn 改变指向到 obj，并执行
* 4、最后返回 obj
 
### 例子
#### 类对象
```js
function User(userAge, userName) {
    this.userAge = userAge
    this.userName = userName
}
User.prototype.showInfo = function() {
    console.log('this.userAge :', this.userAge)
    console.log('this.userName :', this.userName)
}
```

#### 模拟new运算符功能函数
```js
function myNew() {
   let obj = {}
    let arg = Array.prototype.slice.call(arguments, 1)
    obj.__proto__ = Fn.prototype
    obj.__proto__.constructor = Fn
    Fn.apply(obj, arg)
    return obj
};
```

#### 测试
```js
const user = myNew(User,18, '鬼鬼')  
```

### 参考资料
* https://boxuegu.com/ask/detail/12985
* https://dazhuanlan.com/2020/02/03/5e37df6bd16b8/
* https://cnblogs.com/linjunfu/p/10791467.html