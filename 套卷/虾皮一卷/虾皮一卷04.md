3.cookies、sessionStorage和localStorage解释及区别?

#### cookie和session
>cookie和session都是用来跟踪浏览器用户身份的会话方式。

#### 区别：

1、保持状态：cookie保存在浏览器端，session保存在服务器端

#### cookie和session使用方式：
* （1）cookie机制：如果不在浏览器中设置过期时间，cookie被保存在内存中，生命周期随浏览器的关闭而结束，这种cookie简称会话cookie。如果在浏览器中设置了cookie的过期时间，cookie被保存在硬盘中，关闭浏览器后，cookie数据仍然存在，直到过期时间结束才消失。

Cookie是服务器发给客户端的特殊信息，cookie是以文本的方式保存在客户端，每次请求时都带上它

* （2）session机制：当服务器收到请求需要创建session对象时，首先会检查客户端请求中是否包含sessionid。如果有sessionid，服务器将根据该id返回对应session对象。如果客户端请求中没有sessionid，服务器会创建新的session对象，并把sessionid在本次响应中返回给客户端。通常使用cookie方式存储sessionid到客户端，在交互中浏览器按照规则将sessionid发送给服务器。如果用户禁用cookie，则要使用URL重写，可以通过response.encodeURL(url) 进行实现；API对encodeURL的结束为，当浏览器支持Cookie时，url不做任何处理；当浏览器不支持Cookie的时候，将会重写URL将SessionID拼接到访问地址后。

#### 存储内容：
cookie只能保存字符串类型，以文本的方式；session通过类似与Hashtable的数据结构来保存，能支持任何类型的对象(session中可含有多个对象)

#### 存储的大小：
cookie：单个cookie保存的数据不能超过4kb；session大小没有限制。


#### 安全性：
cookie：针对cookie所存在的攻击：Cookie欺骗，Cookie截获；session的安全性大于cookie。

#### 原因如下：   
* （1）sessionID存储在cookie中，若要攻破session首先要攻破cookie；　　　　　　　 
* （2）sessionID是要有人登录，或者启动session_start才会有，所以攻破cookie也不一定能得到sessionID；　　　　　　　
* （3）第二次启动session_start后，前一次的sessionID就是失效了，session过期后，sessionID也随之失效。　　　　
* （4）sessionID是加密的　　
* （5）综上所述，攻击者必须在短时间内攻破加密的sessionID，这很难。

### 应用场景：
#### cookie：
* （1）判断用户是否登陆过网站，以便下次登录时能够实现自动登录（或者记住密码）。如果我们删除cookie，则每次登录必须从新填写登录的相关信息。
* （2）保存上次登录的时间等信息。
* （3）保存上次查看的页面
* （4）浏览计数
#### session：
* （1）Session用于保存每个用户的专用信息，变量的值保存在服务器端，通过SessionID来区分不同的客户。
* （2）网上商城中的购物车
* （3）保存用户登录信息
* （4）将某些数据放入session中，供同一用户的不同页面使用
* （5）防止用户非法登录

### 缺点：
#### cookie：
* （1）大小受限
* （2）用户可以操作（禁用）cookie，使功能受限
* （3）安全性较低
* （4）有些状态不可能保存在客户端。
* （5）每次访问都要传送cookie给服务器，浪费带宽。
* （6）cookie数据有路径（path）的概念，可以限制cookie只属于某个路径下。

 #### session：
* （1）Session保存的东西越多，就越占用服务器内存，对于用户在线人数较多的网站，服务器的内存压力会比较大。
* （2）依赖于cookie（sessionID保存在cookie），如果禁用cookie，则要使用URL重写，不安全
* （3）创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。


### Web Storage
#### sessionStorage：
将数据保存在session对象中。所谓session，是指用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间。session对象可以用来保存在这段时间内所要求保存的任何数据。

#### localStorage：
将数据保存在客户端本地的硬件设备(通常指硬盘，也可以是其他硬件设备)中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。

这两者的区别在于，sessionStorage为临时保存，而localStorage为永久保存。

WebStorage的目的是克服由cookie所带来的一些限制，当数据需要被严格控制在客户端时，不需要持续的将数据发回服务器。

#### WebStorage两个主要目标：
* （1）提供一种在cookie之外存储会话数据的路径。
* （2）提供一种存储大量可以跨会话存在的数据的机制。

 

### 参考资料
* https://blog.csdn.net/qq_41328247/article/details/108523868
