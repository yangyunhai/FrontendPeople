https://mp.weixin.qq.com/s/ztB6RaBNjcjmrhKMHZ6uJQ
Js 事件循环机制
面试官：谈谈你对异步回调、promise、async/await的理解？
src和href的关系


2.1 参考地址  https://www.jianshu.com/p/1a35857c78e5
什么是错误优先的回调函数
错误优先的回调函数用于传递错误和数据。第一个参数始终应该是一个错误对象， 用于检查程序是否发生了错误。其余的参数用于传递数据。
fs.readFile(filePath, function(err, data) {  
    if (err) {
        //handle the error
    }
    // use the data object
});
nodejs的主线程是一个单线程，那么它是怎么实现非阻塞的
libuv来屏蔽不同操作系统的差异，并且通过这个libuv来实现多线程
谈下对nodejs中微任务、宏任务的理解
谈谈对闭包的理解  内层函数能调用外层函数的作用域
说下http1.0和http2.0的区别
http2.0支持websocket长链接；http2.0支持头部信息压缩
HTML5特性
.CSS3特性
JS加载阻塞DOM渲染问题，怎么解决
https://blog.csdn.net/aizou6838/article/details/101664336
Flex实现三栏等宽布局
http状态码
.get和post区别
二叉树怎么遍历（编程）
https://www.cnblogs.com/du001011/p/11229170.html
二叉树的遍历分为4种：先序遍历、中序遍历、后序遍历、层序遍历
Webpack是什么、作用
webpack是一个js静态模块打包工具，很多文件打包成一个文件，从而可以减少请求次数，优化网页性能。
递归爆栈问题，如何解决
17.1为什么会爆栈 因为缺少终止的条件，所以要加终止条件
17.2看有没有可以替代的方式 比如用while循环代替（也要考虑终止条件）
es5、es6、es7、es8区别
 https://www.jianshu.com/p/909405b7aae4
18.1 es5 扩展了Object、Array、Function
18.2 es6  类、模块化、剪头函数、函数参数默认值、模版字符串、解构赋值、延展操作符、对象属性简写（对象扩展，还有计算属性）、Promise、let、const（块级作用域）、新数据结构Set、Map、Symbol
18.3 es7 Array.prototype.includes()、指数操作符
18.4 es8 async/await、Object.values()、Object.entries()、String padding、函数参数列表结尾允许逗号、Object.getOwnPropertyDescriptors()等
javascript有哪些常用的数据类型 string、number、boolean、null、undefined和object
==和===区别 比较的过程不一样，前者类型不同，会转换类型再比较；后者类型不同就不比较了
好了，今天先发这20道题，部分题目是自己平时收藏的经典面试题，大部分是字节的面试题，知悉！

https://www.cnblogs.com/kaima/p/bytedance_interview.html