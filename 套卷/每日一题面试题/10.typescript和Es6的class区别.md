>TypeScript是ES6的超集。至于需不需要使用，在于你所需要的场景。

### 相同点
1. 都是采用extends语法进行继承
2. 在constructor中都需要首先使用super()调用父类构造函数，然后才能获取父类的属性
3. 最终都是通过ES5的原型链进行继承

### 不同点
1. typescript class中有属性字段有private、protected、readonly等修饰符
2. typescript constructor构造函数参数必须定义所属类型
3. typescript class中可以存着静态方法
4. typescript class方法参数会类型验证
5. es6中， 只有静态方法，没有静态属性。

* public 共有的。 类的内外都可以使用。
* protected 受保护的。 类的内部使用，继承的子类中使用。
* private 私有的。 类的内部使用。

### ES6
```js
class UserBase {
  constructor (color) {
    this.color = color;
  }
  static showColor () {
    //color ==>undefined
    console.log(`我是一个有颜色的公有静态函数,看看我的颜色${this.color}`)
  }
}

class User extends UserBase {
  constructor (userName, userAge) {
    super("yellow");
    this.userName=userName;
	this.userAge=userAge;
  }
  showInfo(){
     console.log(`名字${this.userName}今年：${this.userAge}`)
  }
}

const user=new User("鬼鬼",18);
user.showInfo();//实例函数
User.showColor()//静态函数
```


### typescript
```js
class UserBase{
    static color="yellow";//静态属性
    static showColor() {//静态方法
      console.log(`我是一个有颜色的公有静态函数,看看我的颜色${this.color}`)
    }
}

class User extends UserBase{
    private userName: string;
    private userAge: number;
    constructor(userName : string,userAge:number){
		super();//必须调用super才能继承showColor函数
        this.userName=userName;
		this.userAge=userAge;
    }
    private showInfo ():void{
      console.log(`名字${this.userName}今年：${this.userAge}`)
    }
}

const user=new User("鬼鬼",18);
user.showInfo();//实例函数
User.showColor();//静态函数
```