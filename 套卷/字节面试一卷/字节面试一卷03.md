## 面试官：src和href的区别是什么？
虽然一直在用这两个属性，但是一直没有具体的去区分和了解这两个属性的区别，今天就来看看

href标识超文本引用，用在link和a等元素上，href是引用和页面关联，是在当前元素和引用资源之间建立联系

src表示引用资源，表示替换当前元素，用在img，script，iframe上，src是页面内容不可缺少的一部分。

src是source的缩写，是指向外部资源的位置，指向的内部会迁入到文档中当前标签所在的位置；在请求src资源时会将其指向的资源下载并应用到当前文档中，例如js脚本，img图片和frame等元素。

`<script src="js.js"></script>`当浏览器解析到这一句的时候会暂停其他资源的下载和处理，直至将该资源加载，编译，执行完毕，图片和框架等元素也是如此，类似于该元素所指向的资源嵌套如当前标签内，这也是为什么要把js饭再底部而不是头部。

`<link href="common.css" rel="stylesheet"/>`当浏览器解析到这一句的时候会识别该文档为css文件，会下载并且不会停止对当前文档的处理，这也是为什么建议使用link方式来加载css而不是使用@import。
 

### 补充：link和@import的区别
>两者都是外部引用CSS的方式，但是存在一定的区别：

#### 区别1：
link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
#### 区别2：
link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
#### 区别3：
link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
#### 区别4：
ink支持使用Javascript控制DOM去改变样式；而@import不支持。
### 参考资料
* https://blog.csdn.net/binlety/article/details/81448195