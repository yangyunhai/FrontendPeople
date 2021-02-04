###  1. 使⽤过的`koa2`中间件

###  koa-router
`koa.js`为了保持自身的精简，并没有像`Express.js`自带了路由功能，因此`koa-router`做了很好的补充，作为`koa`星数最多的中间件，`koa-router`提供了全面的路由功能，比如类似`Express`的`app.get/post/put`的写法，URL命名参数、路由命名、支持加载多个中间件、嵌套路由等

###  koa-bodyparser

`koa.js`并没有内置`Request Body`的解析器，当我们需要解析请求体时需要加载额外的中间件，官方提供的`koa-bodyparser`是个很不错的选择，支`持x-www-form-urlencoded`, `application/json`等格式的请求体，但不支持`form-data`的请求体，需要借助 `formidable` 这个库，也可以直接使用 `koa-body` 或 `koa-better-body`

###  koa-views
视图模板渲染中间件，支持`ejs`, `nunjucks`等众多模板引擎。

###  koa-static

用作类似`Nginx`的静态文件服务，在本地开发时特别方便，可用于加载前端文件或后端`Fake`数据，可结合 `koa-compress` 和 `koa-mount` 使用。

###  koa-jwt

`koa-jwt`这个中间件使用`JWT认证`HTTP`请求。

###  koa-logger

`koa-logger`提供了输出请求日志的功能，包括请求的`url`、状态码、响应时间、响应体大小等信息，对于调试和跟踪应用程序特别有帮助


###  2. `koa-body`原理

`koa-body`可以实现文件上传，同时也可以让`koa`能获取`post`请求的参数,

###  3. 介绍自己写过的中间件

###  koa中编写中间件，很简单：

下边，我是写了一个异常捕获中间件，在app.js中使用app.use（）注册即可：

```javascript
// errMiddleWare .js
const errMiddleWare = async (ctx, next) => {
    try {
        // 加上了await ，相当于等所有的中间件都执行完毕后，这个next才会执行
        await next() 
    }catch(err) {
        ctx.body = '服务器异常，请稍后！！！'
    }
}
module.exports = {
    errMiddleWare
}
```


```javascript
//app.js
const App = require('koa') // koa 框架
const { errMiddleWare } = require('./errMiddleWare/errMiddleWare.js')
const app = new App()
app.use(errMiddleWare) // 注册自定义异常捕获中间件
app.litsen(3000)
```

1、异常捕获中间件，如果你想让异常捕获中间件 捕获 全局所有的异常错误，需要放到所有中间件的前边，原因也很简单，加上await next( )，永远会等到下一个中间件执行完毕之后，它才会返回结果，所以全局的异常捕获中间件 只有放在最前边，才会捕获到代码执行过程中的任何错误：

```javascript
app.use(async (ctx, next) => {
    const res = await next()
    console.log('1', res)
})
 
app.use(async (ctx, next) => {
    const res = await next()
    console.log('2', res)
})
```

 输出结果：

![](https://img-blog.csdnimg.cn/20200210215607614.png)

###  4. 有没有涉及到`Cluster`

单个 `Node.js` 实例运行在单个线程中。 为了充分利用多核系统，有时需要启用一组 `Node.js` 进程去处理负载任务。

```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`主进程 ${process.pid} 正在运行`);

  // 衍生工作进程。
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`工作进程 ${worker.process.pid} 已退出`);
  });
} else {
  // 工作进程可以共享任何 TCP 连接。
  // 在本例子中，共享的是 HTTP 服务器。
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('你好世界\n');
  }).listen(8000);

  console.log(`工作进程 ${process.pid} 已启动`);
}
```

`cluster` 模块可以创建共享服务器端口的子进程。
###  5. 介绍`pm2`

`PM2`是`node`进程管理工具，可以利用它来简化很多`node`应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单
###  6. `master`挂了的话`pm2`怎么处理
###  介绍
你也许知道，Node.js是一个运行在名叫V8的JavaScript引擎的平台系统。V8本身是单线程运行的，并没有充分利用多核系统能力。
(注：Node执行JS代码运行在V8上，是单线程，但并非真正的单线程架构)
###  Node.js的集群模式
幸运的是，Node.js提供了集群模块，简单讲就是复制一些可以共享TCP连接的工作线程。
###  工作原理
集群模块会创建一个master主线程，然后复制任意多份程序并启动，这叫做工作线程。
工作线程通过 IPC 频道进行通信并且使用了 Round-robin algorithm 算法进行工作调度以此实现负载均衡。

Round-robin调度策略主要是master主线程负责接收所有的连接并派发给下面的各个工作线程。
如何使用
###  下面是一个很常见的例子：
```javascript
var cluster = require('cluster');  
var http    = require('http');  
var os      = require('os');
var numCPUs = os.cpus().length;
if (cluster.isMaster) {  
  // Master:
  // Let's fork as many workers as you have CPU cores
  for (var i = 0; i < numCPUs; ++i) {
    cluster.fork();
  }
} else {
  // Worker:
  // Let's spawn a HTTP server
  // (Workers can share any TCP connection.
  //  In this case its a HTTP server)
  http.createServer(function(req, res) {
    res.writeHead(200);
    res.end("hello world");
  }).listen(8080);
}
```

你可以不受CPU核心限制的创建任意多个工作线程。

使用原生方法有些麻烦而且你还需要处理如果某个工作线程挂掉了等额外的逻辑。
(注：通过fork()复制的进程都是独立的进程，有着全新的V8实例)

PM2的方式

PM2内置了处理上述的逻辑，你不用再写这么多繁琐的代码了。

只需这样一行：
`$ pm2 start app.js -i 4`

`-i <number of workers>` 表示实例程序的个数。就是工作线程。

如果i为0表示，会根据当前CPU核心数创建

![](https://cdn.nlark.com/yuque/0/2020/webp/1440780/1593997141390-1ea84b29-7be8-4e57-b64a-67e3942a5b91.webp)
###  保持你的程序不中断运行
如果有任何工作线程意外挂掉了，PM2会立即重启他们，当前你可以在任何时候重启，只需：
![](https://cdn.nlark.com/yuque/0/2020/webp/1440780/1593997145821-7c842e07-c86b-4187-a0a1-e456a8a670bc.webp)
###  实时调整集群数量
你可以使用命令 pm2 scale <app name> <n> 调整你的线程数量，
如 pm2 scale app +3 会在当前基础上加3个工作线程。

![](https://cdn.nlark.com/yuque/0/2020/webp/1440780/1593997150454-73b171c0-f357-41c0-9b5e-f9511577a2c1.webp)


###  在生产环境让你的程序永不中断
PM2 reload <app name> 命令会一个接一个的重启工作线程，在新的工作线程启动后才结束老的工作线程。

这种方式可以保持你的Node程序始终是运行状态。即使在生产环境下部署了新的代码补丁。

也可以使用gracefulReload命令达到同样的目的，它不会立即结束工作线程，而是通过IPC向它发送关闭信号，这样它就可以关闭正在进行的连接，还可以在退出之前执行一些自定义任务。这种方式更优雅。
```javascript
process.on('message', function(msg) {  
  if (msg === 'shutdown') {
    close_all_connections();
    delete_cache();
    server.close();
    process.exit(0);
  }
});
```
###  结论
Cluster集群模式非常强悍有用，此功能是在Node 0.10.x 是实验功能，在0.11.x 之后才作为正式发布。

强烈建议你使用最新版本的Node.js和PM2。
###  7. 如何和`MySQL`进行通信
>这道题，按照现在的技术发展其实基本不会问了，因为现在肯定是不用原生的node+mysql结合来开发了的，肯定是用koa或者egg这种二次封装过的框架来写了的，所以答案也只能算给我们做个了解吧！

Node.js与MySQL交互操作有很多库，常用最多的是mysql模块，首先需要进行npm安装：

`npm install mysql`

mysql数注意：安装前先把目录cd到node.exe所在目录下，这样执行安装命令时，会找到目录下node_modules，并安装在此目录下，否则使用mysql时，你会出现 Error: Cannot find module 'mysql'
###  链接mysql的流程
```javascript
var mysql  = require('mysql');  //调用MySQL模块
//创建一个connection
var connection = mysql.createConnection({     
  host     : '192.168.0.200',       //主机
  user     : 'root',               //MySQL认证用户名
  password : 'abcd',        //MySQL认证用户密码
  port: '3306',                   //端口号
}); 
//创建一个connection
connection.connect(function(err){
    if(err){        
          console.log('[query] - :'+err);
        return;
    }
      console.log('[connection connect]  succeed!');
});  

//执行SQL语句
connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) { 
     if (err) {
             console.log('[query] - :'+err);
        return;
     }
     console.log('The solution is: ', rows[0].solution);  
});  

//关闭connection
connection.end(function(err){
    if(err){        
        return;
    }
      console.log('[connection end] succeed!');
});
```
数据库连接参数说明：

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2010192347_1603122472067.png)

###  MYSQL CURD操作

###  添加
```javascript
var mysql = require('mysql');
var DATABASE = "seckill";
var TABLE="seckill"
var connection = mysql.createConnection({
    host:'127.0.0.1',
    user:'root',
    password:'12345',
    port:'3306',
    database: DATABASE
});
connection.connect();
var addVip = 'insert into seckill(name,number) values(?,?)';
var param = ['100元秒杀家教机',100];
connection.query(addVip, param, function(error, result){
    if(error)
    {
        console.log(error.message);
    }else{
        console.log('insert id: '+result.insertId);
    }
});
connection.end();
```
###  删除
```javascript
ar mysql = require('mysql');
var DATABASE = "node";
var TABLE="seckill"
var connection = mysql.createConnection({
    host:'127.0.0.1',
    user:'root',
    password:'12345',
    port:'3306',
    database: DATABASE
});

connection.connect();
var addVip = 'delete from seckill where seckill_id = 1005';
connection.query(addVip, function(error, result){
    if(error)
    {
        console.log(error.message);
    }else{
        console.log('affectedRows: '+result.affectedRows);
    }
});
connection.end();
```
###  查找
```javascript
var mysql = require("mysql");
var DATABASE = "node";
var TABLE="seckill"
var connection = mysql.createConnection({
    host:'127.0.0.1',
    user:'root',
    password:'12345',
    port:'3306',
});

connection.connect();
connection.query('use '+DATABASE);
connection.query('select * from '+TABLE, function(error, results, fields){
    if (error) {
        throw error;
    }
    if (results) {
        for(var i = 0; i < results.length; i++)
        {
            console.log('%s\t%s',results[i].name,results[i].end_time);
        }
    }
});
connection.end();
```

###  修改
```javascript
var mysql = require('mysql');
var DATABASE = "seckill";
var connection = mysql.createConnection({
    host:'127.0.0.1',
    user:'root',
    password:'12345',
    port:'3306',
    database: DATABASE
});

connection.connect();
var userSql = "update seckill set number = number-1 where seckill_id = ?";
var param = [1000, 2];
connection.query(userSql, param, function (error, result) {
    if(error)
    {
        console.log(error.message);
    }else{
        console.log('affectedRows: '+result.affectedRows);
    }
});
connection.end();
```

###  结束连接

结束连接其实有两种方法end()，destory()；  

>end()方法在queries都结束后执行，end()方法接收一个回调函数，queries执行出错，仍然后结束连接，错误会返回给回调函数err参数，可以在回调函数中处理！ destory() 比较暴力，没有回调函数，即刻执行，不管queries是否完成！

###  连接池Pooling connections

1.连接池的创建，使用createPool方法，options和createConntion一致，可以监听connection事件。 比较暴力，没有回调函数，即刻执行，不管queries是否完成！
```javascript
var mysql = require('mysql');
//创建连接池
var pool  = mysql.createPool({
  host     : '192.168.0.200',
  user     : 'root',
  password : 'abcd'
});
//监听connection事件
pool.on('connection', function(connection) {  
    connection.query('SET SESSION auto_increment_increment=1'); 
});
连接池可以直接使用，也可以共享一个连接或管理多个连接（引用官方示例）  
//直接使用
pool.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
  if (err) throw err;
  console.log('The solution is: ', rows[0].solution);
});
//共享
pool.getConnection(function(err, connection) {
  // connected! (unless `err` is set)
});
```
###  连接池配置选项 ：　
waitForConnections 　　当连接池没有连接或超出最大限制时，设置为true且会把连接放入队列，设置为false会返回error 　　connectionLimit 　　连接数限制，默认：10 　　queueLimit 　　最大连接请求队列限制，设置为0表示不限制，默认：0
调用connection.release()方法，会把连接放回连接池，等待其它使用者使用!  在实际开发过程中，应该还是使用连接池的方式比较好！
断线重连
数据库可以因为各种原因导致连接不上，这种就必须有重连接机制！主要判断errorcode:PROTOCOL_CONNECTION_LOST 
1.首先去数据库服务器停止MySQL服务2.运行断线重连代码 3.去数据为服务器，开启mysql服务器，再看

###  看执行结果
###  断线重连示例源码：
```javascript
var mysql = require('mysql');
var db_config = {
  host     : '192.168.0.200',       
  user     : 'root',              
  password : 'abcd',       
  port: '3306',                   
  database: 'nodesample'  
};
var connection;
function handleDisconnect() {
  connection = mysql.createConnection(db_config);                                              
  connection.connect(function(err) {              
    if(err) {                                     
      console.log("进行断线重连：" + new Date());
      setTimeout(handleDisconnect, 2000);   //2秒重连一次
      return;
    }         
     console.log("连接成功");  
  });   

  connection.on('error', function(err) {
    console.log('db error', err);
    if(err.code === 'PROTOCOL_CONNECTION_LOST') { 
      handleDisconnect();                         
    } else {                                      
      throw err;                                 
    }
  });
}
handleDisconnect()
```
###  防止SQL注入
防止SQL注入，可以使用pool.escape()和connect.escape() 
```javascript
var mysql = require('mysql');
var pool = mysql.createPool({
    host: '192.168.0.200',     
    user: 'root',
    password:'abcd',
    port:'3306',
    database:'nodesample'
});
pool.getConnection(function(err,connection){
    connection.query('SELECT * FROM userinfo WHERE id = ' + '5 OR ID = 6',function(err,result){
        //console.log(err);
        console.log(result);
        connection.release();
    });
    connection.query('SELECT * FROM userinfo WHERE id = ' + pool.escape('5 OR ID = 6') ,function(err,result){
        //console.log(err);
        console.log(result);
        connection.release();
    });
})
```
结果可以看出，第1个query拼接条件可以被执行，而通过escape方法转义后的忽略了后面的拼接的部分！
大家可以看到我前面用的？占位的方式，简单的试了一下，好处并没有这种危险，这里就不提供示例了，在我上面提供的代码上改一下就可以试出来^_^!

###  mysql.escapeId(identifier)
如果不能信任由用户提示的SQL标识符（数据库名，列名，表名），可以使用此方法，官方提供有示例（最常见的是通过列名来排序什么的...） 　　

###  mysql.format
准备查询，该函数会选择合适的转义方法转义参数



###  8. 服务端渲染`SSR`
###  服务端渲染是什么
我最开始接触是在Vue的官网上，开始是作为一个小节出现，现在已经是个专门的大章节来专门讲Vue服务端渲染的内容。

服务端渲染 简单来说就是在服务器上把数据和模板拼接好以后发送给客户端显示。

回顾下前端的历史，最开始的站点是简单的静态网站。后端大哥把.html文件推送给用户，用户浏览器解析这些字符串进行显示。那个时候就是 服务端渲染 。可是后来由于网站内容越来越复杂、特效越来越炫酷，这种‘兼职’状态已经不能满足需求，细分之下的前端出现了。

随后为了方便的开发，开始提倡 前后端分离，大家各做个的，彼此之间通过基于HTTP的各种API协作，变成了数据动态生成的新一代站点。

再后来出现了Vue等三大MV*框架，网站做成了SPA应用，解决了很多问题的同时也带来了新问题，其中最突出的两个：难以SEO和首屏加载缓慢。

###  服务端渲染解决了什么
####  难以SEO
SPA网站们不仅数据是动态生成的，连大部分DOM节点都是动态生成的，后台只提供一个基本模板，而内容需要等到各种JS文件在客户端下载运行完成以后才能显示。

而搜索引擎目前并不会去下载这些JS文件来爬数据（据说 Google 已经有了这项技术并在使用，百度也能这样做但没做），那么在搜索引擎改变策略前，总得想点办法。

时尚就是轮回，现在前端竟然也有这个现象...那么大神们想到了办法：那就让我们回到老路上吧。
得益于Node.js的出现，不需要后台做太多，把数据和模板在中间服务器上进行组装，再发送给客户端。这样的模式解决了问题又没有让大家倒退回去，

####  首屏加载缓慢
随着前端的发展，业务逻辑越来越复杂，代码也越来越厚，各种JS文件越来越大，当一个网页打开，所有东西都下载完页面能打开，白屏时间越来越长。

为了解决这个问题，代码模块化 和 按需加载、占位图 和 预展示 纷纷开始应用，从不同的角度削减了问题程度。但是服务端渲染同样也是解决这个问题的角度之一，由于不少资源在中间服务器上进行拼接，节省了客户端的不少时间，效果也很不错。


####  服务端渲染有什么缺点
在解决问题的同时同样也有一些成本是必须要考虑的。

首先是 技术成本，中间增加了这些环节当然要多更多的时间或更多的人来完成，并且还有不少坑要踩。

然后很多计算从客户端移到服务器上，对服务器的压力增加，特别是高并发时会给服务器的 CPU 带来更大的负载。

####  常见的解决方案
Nuxt.js

###  结语
我个人觉得用不用、怎么用依然得看需求和取舍，技术是工具，主要还是看人。