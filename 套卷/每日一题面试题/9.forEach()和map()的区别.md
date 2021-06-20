### 相同点：
* 都是循环遍历数组中的每一项
* 每次执行匿名函数参数一样，分别是item（当前每一项）、index（索引值）、arr（原数组）
* 只能遍历数组
* 匿名函数中的this都是指向window；
* 两种方法都不能用 break 中断
```js
const users = ["鬼鬼", "刘亦菲", "周星驰"];

users.forEach((item, index, arr) => {
    console.log(item, index, arr);
},this);
```

```js
const users = ["鬼鬼", "刘亦菲", "周星驰"];

users.map((item,index,arr)=>{
   console.log(item,index,arr)
},this)
```
### 不同点：
* map()速度比forEach()快；
* map()会返回一个新数组，不对原数组产生影响；
* map()方法输出可以与其他方法(如reduce()、sort()、filter())链接在一起

### 性能对比
```js
const users=new Array(10000).fill("鬼鬼");
// 1. forEach()
console.time("forEach");
const newArray = [];
users.forEach((item) => {
  newArray.push(item)
});
console.timeEnd("forEach");

// 2. map()
console.time("map");
const newArray2 = users.map ((item) => {
  return item
});
console.timeEnd("map");
```
>差距还是挺大的

### 参考资料
* https:/cnblogs.com/kaiqinzhang/p/11496151.html