2.new的过程发生了什么?
代码实例
```js
function User(userName) {
    this.userName = userName;
}
const user = new User("鬼哥");
```

1. 创建空对象；
```js
var obj = {};
```
2. 设置新对象的 constructor 属性为构造函数的名称，设置新对象的__proto__属性指向构造函数的 prototype 对象；
```js
obj.__proto__ = User.prototype;
```
3. 使用新对象调用函数，函数中的 this 被指向新实例对象：
```js
User.call(obj); //{}.构造函数();
```
4. 如果无返回值或者返回一个非对象值，则将新对象返回；如果返回值是一个新对象的话那么直接直接返回该对象。
```js
if (typeof(result) == "object") {
    user = result;
} else {
    user = obj;
}
```

### 参考资料
* https://my.oschina.net/qiilee/blog/4915319
* https://developer.aliyun.com/ask/258926