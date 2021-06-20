### 继承的含义：
>继承是面向对象编程中的一个重要概念，通过继承可以使子类的实例拥有在父类中定义的属性和方法。

### 1、原型链继承
```js
function UserBase(){
}
function User(){
}
User.prototype = new UserBase();
```
>将父类的实例作为子类的原型。

* （1）不能向构造函数传参，无法实现多继承
* （2）来自原型对象的引用属性是所有实例共享的

### 2、构造继承
实际上使用父类的构造函数来增强子类，等于是把父类的构造函数复制给子类。
```js
function UserBase(){
}
function User(userName) {
    UserBase.call(this);
    this.userName = userName;
}
let user = new User("鬼鬼")
user.userName;
```
##### 优点：
* （1）可以向构造函数传参数
* （2）可以实现多继承，多call几个

##### 缺点：
* （1）无法实现函数复用
* （2）只能继承父类的属性和方法，不能继承父类的原型

### 3、实例继承
为父类实例添加新属性，作为子类实例返回。
```js
function UserBase(){
}
function User(userName) {
  let userBase = new UserBase();
  userBase.userName = userName;
  return userBase;
}
let user = new User("鬼鬼")
user.userName;
```
* 缺点：无法实现多继承

### 4、拷贝继承
```js
function UserBase(userName){
}
UserBase.prototype.showInfo = function(){
	console.log(this.userName)
}
function User(userName) {
 let userBase = new UserBase();
 for (let attr in userBase) {
   User.prototype[attr] = userBase[attr];
 }
 this.userName = userName;
}

let user = new User("鬼鬼")
user.showInfo();
```
* 优点：支持多继承
* 缺点：占用内存高，因为要用for in循环来拷贝父类属性／方法

不可枚举方法拷贝不了

### 5、组合继承
通过调用父类构造函数，继承了父类的属性，并保留了传参的优点。

然后再将父类实例作为子类原型，实现了函数复用。
```js
function UserBase(userName){
	this.userName = userName
}
UserBase.prototype.showInfo = function(){
	console.log(this.userName)
}
function User (userName){
    //call方式 
	UserBase.call(this,userName)
     //apply方式 
    UserBase.apply(this,[userName])
}
User.prototype = new UserBase()
let user = new User("鬼鬼")
user.showInfo(); 
```
##### 优点：
* （1）继承父类的属性和方法，也继承了父类的原型
* （2）可传参，函数可复用

##### 缺点：
调用了两次父类构造函数


### 6、寄生组合继承
通过寄生的方式，去掉了父类的实例属性，在调用父类构造函数时，

就不会初始化两次实例方法，避免了组合继承的缺点
```js
function UserBase(userName){
	this.userName = userName
}
UserBase.prototype.showInfo = function(){
	console.log(this.userName)
}
function User (userName){
	UserBase.call(this,userName)
}
User.prototype = Object.create(UserBase.prototype)
User.prototype.constructor = User
let user = new User("鬼鬼")
user.showInfo(); 
```

#### 7、Class继承
```js
class UserBase{
	constructor(userName){
		this.userName = userName
	}
	showInfo(){
		console.log(this.userName)
	}
}
class User extends UserBase{
	constructor(value){
		super(value) 
	}
}
var user = new User("鬼鬼")
user.showInfo();
```
### 参考资料
* https://blog.csdn.net/guoqing2016/article/details/106418081/
* http://www.bubuko.com/infodetail-2556919.html