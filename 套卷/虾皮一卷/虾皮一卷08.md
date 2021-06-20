### 面试官：聊聊你对webpack相关的了解

### webpack和grunt以及gulp有什么不同？
>grunt和gulp是基于任务运行工具。 我们需要把我们要做的事，分配成各种各样的
任务，grunt和gulp它会自动执行指定的任务，像流水线，把资源放上去，通过不同的插件进行加工，它的插件非常丰富，能够帮我们打造各种的工作流。

webpack是模块打包器，webpack是所有的文件都当作模块来处理，也就是说webpack和grunt以及gulp
是完全不同的两类工具，

### 什么是bundle? 什么是chunk？ 什么是module？
* chunk: 代码块 一个chunk可能有很多的模块构造 用于代码的合并和分割
* module：模块 在webpack世界中，一切都是模块 一个文件就是一个模块 webpack会从entry开始查找出项目所有的依赖的模块
* bundle: webpack打包后生成的文件

### 什么是loader ? 什么是Plugin ? loader和plugin有什么区别？
webapck默认只能打包JS和JOSN模块，要打包其它模块，需要借助loader，loader就可以让模块中的内容转化成webpack或其它laoder可以识别的内容。

loader就是模块转换化，或叫加载器。不同的文件，需要不同的loader来处理。

plugin是插件，可以参与到整个webpack打包的流程中，不同的插件，在合适的时机，可以做不同的事件。

### 常见的loader有哪些？ 你用的loader有哪些？它们有解决什么问题？
* css-loader:
加载css模块 转换成模块化的css 让webpack识别它
* style-loader:
把css代码注入到js中，通过DOM操作去加载CSS 把css代码放到了header标签中style标签中
* babel-loader
把JS高阶转成JS低阶 pollyfill
* eslint-loader:
检查JS代码是否符合某个规则
* file-loader:
把文件输出到一个文件夹中，在代码中通过URL引用输出的文件
* url-loader:
和file-loader类似，比file-loader强大一个，让小图片直接生成base64
* less-loader:
把less代码转化css代码
* html-loader:
处理html模块中的插件图片等

### webpack中都有哪些插件，这些插件有什么作用？
* html-webpack-plugin
自动创建一个HTML文件，并把打包好的JS插入到HTML文件中
* clean-webpack-plugin
在每一次打包之前，删除整个输出文件夹下所有的内容
* mini-css-extrcat-plugin
抽离CSS代码，放到一个单独的文件中
* optimize-css-assets-plugin
压缩css

### loader和plugin有什么区别？
loader 叫 加载器 或 转换器。

webpack中一切都是模块，但是webpack默认只能处理js和json模块，如果你想处理非JS模块，就需要借助loader。

让loader帮我们处理。Loader的作用是让webpack可以处理非JS模块 loader会把非JS模块中的内容转成新的内容。

plugin 插件 扩展webpack的功能，让webpack更加强大，在webpack构建的生命周期中，可以执行不同的插件，影响输出的结果。

### 什么是同源策略？
同源策略是为了保证用户信息安全，是浏览器的机制，防止恶意的网站窃取别人的数据，只允许访问来自同一个站点的资源

#### 同源是指：
协议相同，域名相同，端口相同。这三者相同就是同源。如果不同源，浏览器就会作出限制。

### 参考资料
* https://blog.csdn.net/weixin_46628546/article/details/108954255