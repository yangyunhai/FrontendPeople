
##### 1.`null`，`undefined`的区别？
`null`表示一个对象被定义了，但存放了空指针，转换为数值时为0。

`undefined`表示声明的变量未初始化，转换为数值时为NAN。

`typeof(null)` -- object;

`typeof(undefined)` -- undefined


##### 2.`CSS3`新增伪类有那些？

#### p:first-of-type 
选择属于其⽗元素的⾸个 `<p>` 元素的每个 `<p>` 元素。
#### p:last-of-type 
选择属于其⽗元素的最后 `<p>` 元素的每个 `<p>` 元素。
#### p:only-of-type 
选择属于其⽗元素唯⼀的 `<p>` 元素的每个 `<p>` 元素。
#### p:only-child 
选择属于其⽗元素的唯⼀⼦元素的每个 `<p>` 元素。
#### p:nth-child(2) 
选择属于其⽗元素的第⼆个⼦元素的每个 `<p>` 元素。
#### :after 
在元素之前添加内容,也可以⽤来做清除浮动。
#### :before 
在元素之后添加内容。
#### :enabled 
已启⽤的表单元素。
#### :disabled 
已禁⽤的表单元素。
#### :checked 
单选框或复选框被选中。



##### 3.`div`水平垂直居中几种方式
```HTML
<div class="parent">
    <div class="child"></div>
</div>
```

1）使用position + transform，不定宽高时

```CSS
.parent{
    position: relative;
}
.child{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
}
```

2）使用position + transform，有宽高时


```CSS
.parent{
    position: relative;
}
.child{
    width: 100px;
    height: 100px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -50px;
    margin-top: -50px;
}
```
3）使用flex
```CSS
.parent{
    display: flex;
    align-items: center;
    justify-content: center;
}
```
或者
```CSS
.parent{
    display: flex;
    align-items: center;
}
.child{
    margin: 0 auto;
}
```
或者
```CSS
.parent{
    display: flex;
}
.child{
    margin: auto;
}
```

4）使用position
```CSS
.parent{
    position: relative;
}
.child{
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

5）使用flex;
```CSS
div.parent{
  display:flex;
}
div.child{
  margin:auto;
}
```

##### 4.什么是`BFC`

`BFC`，也叫块级格式化上下文，它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

具有 `BFC` 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素。

通俗一点来讲，可以把 `BFC` 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

此外，BFC 具有普通容器所没有的一些特性。诸如：同一个 `BFC` 下外边距会发生折叠、`BFC` 可以包含浮动的元素（清除浮动）、`BFC` 可以阻止元素被浮动元素覆盖。

只要元素满足下面任一条件即可触发 `BFC` 特性：

* body 根元素
* 浮动元素：float 除 none 以外的值
* 绝对定位元素：position (absolute、fixed)
* display 为 inline-block、table-cells、flex
* overflow 除了 visible 以外的值 (hidden、auto、scroll)


##### 5.`CSS`单位有哪些？

px、%、em、rem、v系单位（vw、vh、vmin、vmax）、单位运算calc()

##### 6.如何减少回流，重排?

浏览器本身的优化策略：

浏览器会维护1个队列，把所有会引起回流、重绘的操作放入这个队列，等队列中的操作到了一定的数量或者到了一定的时间间隔，浏览器就会`flush`队列，进行一个批处理。这样就会让多次的回流、重绘变成一次回流重绘。但有时候我们写的一些代码可能会强制浏览器提前flush队列，这样浏览器的优化可能就起不到作用了。当你请求向浏览器请求一些 `style`信息的时候，就会让浏览器`flush`队列。

减少对`render tree`的操作（合并多次多DOM和样式的修改），并减少对一些`style`信息的请求，尽量利用好浏览器的优化策略

1、将多次改变样式属性的操作合并成一次操作。

2、将需要多次重排的元素，`position`属性设为`absolute`或`fixed`，这样此元素就脱离了文档流，它的变化不会影响到其他元素。例如有动画效果的元素就最好设置为绝对定位。

3、在内存中多次操作节点，完成后再添加到文档中去。例如要异步获取表格数据，渲染到页面。可以先取得数据后在内存中构建整个表格的`html`片段，再一次性添加到文档中去，而不是循环添加每一行。

4、由于`display`属性为`none`的元素不在渲染树中，对隐藏的元素操作不会引发其他元素的重排。如果要对一个元素进行复杂的操作时，可以先隐藏它，操作完成后再显示。这样只在隐藏和显示时触发2次重排。 

5、在需要经常取那些引起浏览器重排的属性值时，要缓存到变量。

##### 7.什么是`1px`像素问题，如何解决?

##### 原因：

我们做移动端页面时一般都会设置`meta`标签`viewport`的`content=width=device-width`，这里就是把html视窗大小设置等于设备的大小，大多数手机的屏幕设备宽度都差不多，以`iphoneX`为例，屏幕宽度`375px`。

而`UI`给设计图的时候基本上都是给的二倍图甚至三倍图，假设设计图是`750px`的二倍图，在`750px`上设计了`1px`的边框，要拿到`375px`宽度的手机来显示，就相当于缩小了一倍，所以1px的边框要以0.5px来呈现才符合预期效果，然而`css`里最低只支持1px大小，不足`1px`就以`1px`显示，所以你看到的就显得边框较粗，实际上只是设计图整体缩小了，而边框粗细没有跟着缩小导致的。（`ps：ios`较新版已经支持`0.5px`了，这里暂且忽略）

简而言之就是：多倍的设计图设计了`1px`的边框，在手机上缩小呈现时，由于`css`最低只支持显示`1px`大小，导致边框太粗的效果。


##### 解决方法:

`transform: scale(0.5)` 方案 - 推荐: 很灵活

1.) 设置`height: 1px`，根据媒体查询结合`transform`缩放为相应尺寸。
```css
div {
    height:1px;
    background:#000;
    -webkit-transform: scaleY(0.5);
    -webkit-transform-origin:0 0;
    overflow: hidden;
}
```

2.) 用`::after`和`::befor`,设置`border-bottom：1px solid #000`,然后在缩放`-webkit-transform: scaleY(0.5)`;可以实现两根边线的需求
```css
div::after{
    content:'';width:100%;
    border-bottom:1px solid #000;
    transform: scaleY(0.5);
}
```

3.) 用`::after`设置`border：1px solid #000`; `width:200%`; `height:200%`,然后再缩放`scaleY(0.5)`; 优点可以实现圆角，京东就是这么实现的，缺点是按钮添加`active`比较麻烦。

```css
.div::after {
    content: '';
    width: 200%;
    height: 200%;
    position: absolute;
    top: 0;
    left: 0;
    border: 1px solid #bfbfbf;
    border-radius: 4px;
    -webkit-transform: scale(0.5,0.5);
    transform: scale(0.5,0.5);
    -webkit-transform-origin: top left;
}
```

#### 方法二:
媒体查询 + transfrom 对方案1的优化
```css
/* 2倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 2.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.5);
        transform: scaleY(0.5);
    }
}
/* 3倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 3.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.33);
        transform: scaleY(0.33);
    }
}
```


#### 8.`display`有哪些值？说明他们的作用
* none 元素不显示，并从文档流中移除。
* inherit 从父元素继承 display 属性的值。
* block块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
* line行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
* list-item像块类型元素一样显示，并添加样式列表标记。


* inline-block	默认宽度为内容宽度，可以设置宽高，同行显示。
* table	作为块级表格来显示。
* flex	弹性元素如何伸长或缩短以适应flex容器中的可用空间。
* grid	网格布局

#### 9.浏览器兼容性有哪些？

1.浏览器默认的 `margin` 和 `padding` 不同

#### 解决：
加一个全局 `*{ margin: 0; padding: 0; }`来统一

2.谷歌中文界面下默认会将小于12px 的文本强制按照`12px`显示

#### 解决：
使用`-webkit-transform:scale(.75)`;收缩的是整个`span`盒子大小，这时候，必须将span准换成块元素。

3.超链接访问过后`hover`样式就不会出现了，被点击访问过的超链接样式不再具有`hover` 和`active` 了

解决：改变`css` 属性的排列顺序`L-V-H-A`

#### 10.`：：before` 和：`after`的区别？

####  `：`
伪类,用于向某些选择器添加特殊的效果。(css2提出) 

#### `：：`
伪元素,用于将特殊的效果添加到某些选择器（css3提出,目的是和伪类区分开）

#### 常见伪类:
* hover, 
* :link, 
* :active, 
* :target, 
* :not(), 
* :focus。 

#### 常见伪元素
* ::first-letter, 
* ::first-line,
* ::before, 
* ::after, 
* ::selection。

所有支持 `::` 语法的浏览器都会支持 `:` 语法，但IE8只支持单冒号。建议使用单冒号，以获得最佳的浏览器支持 `.`

伪类的效果可以通过添加一个实际的类来达到，而伪元素的效果则需要通过添加一个实际的元素才能达到，这也是为什么他们一个称为伪类，一个称为伪元素的原因。

：before /：after这两个伪类下特有的属性 **content** ，用于在 CSS 渲染中向元素逻辑上的头部或尾部添加内容。注意这些添加不会改变文档内容，不会出现在 DOM 中，不可复制，仅仅是在 CSS 渲染层加入。
注：伪元素可用来清除浮动


#### 11.清除浮动的⼏种⽅式，各⾃的优缺点
* ⽗级 `div` 定义 `height`
* 结尾处加空 `div` 标签 `clear:both`
* ⽗级 `div` 定义伪类 `:after` 和 `zoom`
* ⽗级 `div` 定义 `overflow:hidden`
* ⽗级 `div` 也浮动，需要定义宽度 结尾处加 `br` 标签 `clear:both`

#### 12.`css3`有哪些新特性

* 新增各种 `css` 选择器 圆⻆ `border-radius`
* 多列布局
* 阴影和反射 ⽂字特效 `text-shadow`
* 线性渐变 旋转 `transform`

#### 13.`position`的值， `relative`和`absolute`定位原点是?

* absolute ：⽣成绝对定位的元素，相对于 `static` 定位以外的第⼀个⽗元素进⾏定位
* fixed ：⽣成绝对定位的元素，相对于浏览器窗⼝进⾏定位
* relative ：⽣成相对定位的元素，相对于其正常位置进⾏定位
* static 默认值。没有定位，元素出现在正常的流中
* inherit 规定从⽗元素继承 `position` 属性的值

#### 14.`link`与`@import`的区别

1. `link` 是 `HTML` ⽅式， `@import` 是`CSS`⽅式 
2. `link` 最⼤限度⽀持并⾏下载， `@import` 过多嵌套导致串⾏下载，出现 `FOUC` (⽂档样式 短暂失效) 
3. `link` 可以通过 `rel="alternate stylesheet"` 指定候选样式 
4. 浏览器对 `link` ⽀持早于 `@import` ，可以使⽤ `@import` 对⽼浏览器隐藏样式 
5. `@import` 必须在样式规则之前，可以在`css`⽂件中引⽤其他⽂件 
6. 总体来说： `link` 优于 `@import`



#### 15.居中为什么要使用`transform`（为什么不使用`marginLeft`/`Top`）

>我们主要还是从浏览器渲染的性能方面考虑。

#### 原因
1.`marign`：外边距，定义元素周围的空间；简言之，可以改变元素的位移。在浏览器页面渲染的时候，`margin`可以控制元素的位置，也就是说，改变`margin`，就会改变`render tree`的结构，必定会引起页面`layout`回流和`repaint`重绘。

2.`transform`是通过创建一个`RenderLayers`合成层，拥有独立的`GraphicsLayers`。每一个`GraphicsLayers`都有一个`Graphics Context`，其对应的`RenderLayers`会`paint`进`Graphics Context`中。合成器（`Compositor`）最终会负责将由`Graphics Context`输出的位图合并成最终屏幕展示的图案。

#### 结果:

`transform`发生在`Composite Layer`这一步，它所引起的`paint`也只是发生在单独的`GraphicsLayer`中，并不会引起整个页面的回流重绘

因此，从浏览器性能考虑，`transform`会比`margin`更省时间。

#### 16.前端需要注意哪些`SEO`

### 什么是SEO？

`SEO`（Search Engine Optimization）：汉译为搜索引擎优化。是一种方式：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是：为网站提供生态式的自我营销解决方案，让其在行业内占据领先地位，获得品牌收益。

1.合理的`title`，`description`，`keywords`：搜索对三个的权重逐个减小，`title`值强调重点即可，重点词出现不要超过两次，而且要靠前，不同页面的`title`要有所不同。`description`把页面内容高度概括，长度合适，不要过分堆砌关键词，不同页面`description`有所不同，`keywords`列出重要关键词即可

2.语义化`HTML`代码，符合`W3C`规范，语义化代码让搜索引擎容易理解网页

3.重要`HTML`代码放在前面：搜索引擎抓取`HTML`顺序是从上到下，有的搜索引擎对于抓取长度有限制，保证重要内容一定会被抓取

4.重点内容不要用`JS`输出，爬虫不会执行`JS`获取内容

5.少用`iframe`：搜索引擎不会抓取`iframe`中的内容

6.非装饰性图片必须要加`alt`

7.提高网站速度，网站速度是搜索引擎排序的一个重要指标

##### 17.伪元素和伪类的区别是什么？

#### 定义

1.伪元素：伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过`:before`来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

2.伪类：伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过`:hover`来描述这个元素的状态。虽然它和普通的`css`类相似，可以为已有的元素添加样式，但是它只有处于`dom`树无法描述的状态下才能为元素添加样式，所以将其称为伪类。

#### 特点

1.伪元素和伪类都不会出现在源文档或者文档树中

2.伪类允许出现在选择器的任何位置，而一个伪元素只能跟在选择器的最后一个简单选择器后面

3.伪元素名和伪类名都是大小写不敏感的

⑷有些伪类是互斥的，而其它的可以同时用在一个元素上。（在规则冲突的情况下，常规层叠顺序决定结果）。

#### 区别

1.伪类的操作对象是文档树中已有的元素。

2.伪元素则创建了一个文档数外的元素。

3.伪类与伪元素的区别在于：有没有创建一个文档树之外的元素。

#### 一句话通俗描述

`伪元素`和`伪类`都带一个`伪`字，那是他们的共同点，所以，他们的区别就是一个本质上是元素，另一个本质上是类。


##### 18.什么是响应式布局？

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2009121551_1599897102484.1460000021929516[1] "")

通俗来说，响应式布局就是做一个网站同时能兼容多个终端，由一个网站转变成多个网站，为我们大大节省了资源。

响应式界面有四个层次：

1、同一页面在不同大小和比例上看起来都应该是舒适的；

2、同一页面在不同分辨率上看起来都应该是合理的；

4、同一页面在不同操作方式（如鼠标和触屏）下，体验应该是统一的；

5、同一页面在不同类型的设备（手机、平板、电脑）上，交互方式应该是符合习惯的。

在知道了响应式布局是什么后，我们再来简单说一说响应式布局的用法



1、布局及设置`meta`标签

当创建一个响应式网站，或者非响应式网站变成响应式的时候，首先要关注元素的布局；比如，在手机设备上，我们要禁止用户来缩放屏幕。不禁止的话，可能在显示上会造成错位，以及显示的不是手机网站的样式。所以，我们要通过代码来禁止用户在手机端上缩放屏幕，已达到正常的手机网站效果。

2、通过媒体查询来设置样式`media query`

`media query` 是响应式设计的核心，它能够和浏览器进行沟通，告诉浏览器页面如何呈现，假如一个终端的分辨率小于`980px`，那么可以这样写，这个时候的布局会覆盖掉之前设置的布局。

3、设置多种视图宽度

4、响应式的图片

响应式布局的用法在这里就简单介绍一下，对于响应式的深入了解大家可以关注我们的相关栏目进行学习！！！

以上就是响应式布局是什么？响应式布局的介绍的详细内容，希望对你有所帮助，欢迎关注我们，来获取更多的资源。


#### 19.移动端`border 1px` 问题
>加入如下sass文件
```css
/*单条border样式*/
@mixin border-1px ($color, $direction) {
  content: '';
  position: absolute;
  background: $color;
  @if $direction == left {
    left: 0;
    top: 0;
    height: 100%;
    width: 2px;
    transform: scaleX(0.5);
    transform-origin: left 0;
  }
  @if $direction == right {
    right: 0;
    top: 0;
    height: 100%;
    width: 2px;
    transform: scaleX(0.5);
    transform-origin: right 0;
  }
  @if $direction == bottom {
    bottom: 0;
    left: 0;
    width: 100%;
    height: 2px;
    transform: scaleY(0.5);
    transform-origin: 0 bottom;
  }
  @if $direction == top {
    top: 0;
    left: 0;
    width: 100%;
    height: 2px;
    transform: scaleY(0.5);
    transform-origin: 0 top;
  }
}

/*四条border样式*/
@mixin all-border-1px ($color, $radius) {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  border: 2px solid $color;
  border-radius: $radius * 2;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 200%;
  height: 200%;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
  -webkit-transform-origin: left top;
  transform-origin: left top;
}
```

##### 20.为什么图片在安卓上，有些设备模糊问题

用同等比例的图片在PC机上很清楚，但是手机上很模糊，原因是什么呢？

经过研究，是`devicePixelRatio`作怪，因为手机分辨率太小，如果按照分辨率来显示网页，这样字会非常小，所以苹果当初就把`iPhone 4`的`960*640`分辨率，在网页里只显示了`480*320`，这样`devicePixelRatio＝2`。现在`android`比较乱，有`1.5`的，有`2`的也有`3`的。

想让图片在手机里显示更为清晰，必须使用2x的背景图来代替img标签（一般情况都是用2倍）。例如一个`div`的宽高是`100*100`，背景图必须得`200*200`，然后`background-size:contain`;，这样显示出来的图片就比较清晰了。

代码可以如下：
```css
background:url(../images/icon/all.png) no-repeat center center;
-webkit-background-size:50px 50px;

background-size: 50px 50px;
display:inline-block; 
width:100%; 
height:50px;
```
或者指定
```css
 background-size:contain;
```

##### 21.移动端提高可点区域范围

移动端的的一些图标如X，可能会设计得比较小，所以点起来会不太好点，因此要提高可点区域范围，可通过增加`padding`，如下代码：

```css
.icon-close{
    position: abosulte;
    right: 0;
    top: 0;
    padding: 20px;
}
```

##### 22.长时间按住页面出现闪退

```css
element {
-webkit-touch-callout: none;
}
```

##### 23.旋转屏幕时，字体大小调整的问题
```css
html, body, form, fieldset, p, div, h1, h2, h3, h4, h5, h6 {
  -webkit-text-size-adjust:100%;
}
```

##### 24.`IOS`中`input`键盘事件`keyup`、`keydown`、`keypress`支持不是很好

问题是这样的，用`input search`做模糊搜索的时候，在键盘里面输入关键词，会通过`ajax`后台查询，然后返回数据，然后再对返回的数据进行关键词标红。用input监听键盘`keyup`事件，在安卓手机浏览器中是可以的，但是在`ios`手机浏览器中变红很慢，用输入法输入之后，并未立刻相应`keyup`事件，只有在通过删除之后才能相应！

#### 解决办法：

可以用`html5`的`oninput`事件去代替`keyup`

<input type="text" id="testInput">
<script type="text/javascript">
  document.getElementById('testInput').addEventListener('input', function(e){
    var value = e.target.value;
  });
</script>

然后就达到类似`keyup`的效果！


#### 25.`li`与`li`之间有看不见的空白间隔是什么原因引起的？有什么解决办法？

>引起这种空白间隔的原因：浏览器的默认行为是把`inline`元素间的空白字符（空格换行`tab`）渲染成一个空格，也就是我们上面的代码

换行后会产生换行字符，而它会变成一个空格，当然空格就占用一个字符的宽度。

#### 解决方案：

方法一： 既然是因为`<li>`换行导致的，那就可以将`<li>`代码全部写在一排，如下

```html
<div class="wrap">
  <h3>li标签空白测试</h3>
  <ul>
  <li class="part1"></li>
  <li class="part2"></li>
  <li class="part3"></li>
  <li class="part4"></li>
  </ul>
</div>
```

再刷新页面看就没有空白了，就是这么神奇~

方法二： 我们为了代码美观以及方便修改，很多时候我们不可能将`<li>`全部写在一排，那怎么办？既然是空格占一个字符的宽度，那我们索性就将

内的字符尺寸直接设为0，将下面样式放入样式表，问题解决。

```css
.wrap ul{
  font-size:0px;
}
```

但随着而来的就是

中的其他文字就不见了，因为其尺寸被设为`0px`了，我们只好将他们重新设定字符尺寸。

方法三： 本来以为方法二能够完全解决问题，但经测试，将`li`父级标签字符设置为0在`Safari`浏览器依然出现间隔空白；既然设置字符大小为0不行，那咱就将间隔消除了，将下面代码替换方法二的代码，目前测试完美解决。同样随来而来的问题是`li`内的字符间隔也被设置了，我们需要将`li`内的字符间隔设为默认

```css
.wrap ul{
    letter-spacing: -5px;
}
```

之后记得设置li内字符间隔

```css
.wrap ul li{
  letter-spacing: normal;
}
``


#### 26. 移动端适配`1px`的问题

### 前言

在移动端web开发中，UI设计稿中设置边框为1像素，前端在开发过程中如果出现border:1px，测试会发现在retina屏机型中，1px会比较粗，即是较经典的移动端1px像素问题。

本文默认你已经对视口、物理像素、逻辑像素、设备像素比、css像素等移动端基本概念已经了解。
产生原因

#### 设备像素比：
dpr=window.devicePixelRatio，也就是设备的物理像素与逻辑像素的比值。
在retina屏的手机上, dpr为2或3，css里写的1px宽度映射到物理像素上就有2px或3px宽度。
例如：iPhone6的dpr为2，物理像素是750（x轴）,它的逻辑像素为375。也就是说，1个逻辑像素，在x轴和y轴方向，需要2个物理像素来显示，即：dpr=2时，表示1个CSS像素由4个物理像素点组成，如下图所示：

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2010211637_1603269450142.640[1])

#### 伪类+transform 实现
原理：把原先元素的border去掉，然后利用:before或者:after重做border，并 transform的scale缩小一半，原先的元素相对定位，新做的border绝对定位。
```css
/*手机端实现真正的一像素边框*/
.border-1px, .border-bottom-1px, .border-top-1px, .border-left-1px, .border-right-1px {
    position: relative;
}

/*线条颜色 黑色*/
.border-1px::after, .border-bottom-1px::after, .border-top-1px::after, .border-left-1px::after, .border-right-1px::after {
    background-color: #000;
}

/*底边边框一像素*/
.border-bottom-1px::after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 1px;
    transform-origin: 0 0;
}

/*上边边框一像素*/
.border-top-1px::after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 1px;
    transform-origin: 0 0;
}

/*左边边框一像素*/
.border-left-1px::after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 1px;
    height: 100%;
    transform-origin: 0 0;
}

/*右边边框1像素*/
.border-right-1px::after {
    content: "";
    box-sizing: border-box;
    position: absolute;
    right: 0;
    top: 0;
    width: 1px;
    height: 100%;
    transform-origin: 0 0;
}

/*边框一像素*/
.border-1px::after {
    content: "";
    box-sizing: border-box;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    border: 1px solid gray;
}


/*设备像素比*/
/*显示屏最小dpr为2*/
@media (-webkit-min-device-pixel-ratio: 2) {
    .border-bottom-1px::after, .border-top-1px::after {
        transform: scaleY(0.5);
    }

    .border-left-1px::after, .border-right-1px::after {
        transform: scaleX(0.5);
    }

    .border-1px::after {
        width: 200%;
        height: 200%;
        transform: scale(0.5);
        transform-origin: 0 0;
    }
}

/*设备像素比*/
@media (-webkit-min-device-pixel-ratio: 3)  {
    .border-bottom-1px::after, .border-top-1px::after {
        transform: scaleY(0.333);
    }

    .border-left-1px::after, .border-right-1px::after {
        transform: scaleX(0.333);
    }

    .border-1px::after {
        width: 300%;
        height: 300%;
        transform: scale(0.333);
        transform-origin: 0 0;
    }
}
```
需要注意`<input type="button">`是没有:before, :after伪元素的

#### 优点：
所有场景都能满足，支持圆角(伪类和本体类都需要加border-radius)。

#### 缺点：
代码量也很大，对于已经使用伪类的元素(例如clearfix)，可能需要多层嵌套。

###  27. 介绍`flex`布局

flex布局可以帮我们快速布局一些区块,实现你想要的效果,不用再去float,position之类的.我们在布局网页的时候很多时候都是一些特殊布局,flex就能帮我快速去布局,不需要去定位.

任何一个盒子都可以指定为flex布局,但是要注意，设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。

![](https://yangyunhaiimagesoss.oss-cn-shanghai.aliyuncs.com/2010211646_1603269990228.640[1])

###  28.`css`方式设置垂直居中

###  Flex布局
```css
display: flex  /*设置Flex */
justify-content: center /* 主轴对齐 */
align-items: center     /* 侧轴 */
```
flex布局是平四开发中非常常用的，虽然面试官想要一大堆方法，但是flex是最常用的，如果对此理解不够深入建议重学一下

###  定位实现

```css
margin:0 auto;  /* 水平居中 */
position: relative; /* 绝对定位 */
top:50%; 
transform:translateY(-50%); 
```

注：这里第一行实现了水平居中， 剩余几行实现了垂直方向的居中，对css3   transform了解不深者建议学习一下相关知识，这里也可以使用margin-top： -w/2前提是需要知道w是多少，n是需要居中元素的宽度, 也可以去掉margin全部用定位实现，代码如下

```css
position: relative;
left: 50%;
top: 50%;
transform: translateX(-50%) translateY(-50%);
```

###  29. `display:none`和`visibility:hidden`的区别?

#### display:none 
隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢， 就当他从来不存在。

#### visibility:hidden
隐藏对应的元素，但是在文档布局中仍保留原来的空间。


### 30.如何解决`a`标点击后`hover`事件失效的问题?

改变a标签css属性的排列顺序

只需要记住LoVe HAte原则就可以了：
```css
link→visited→hover→active
```

比如下面错误的代码顺序：
```css
a:hover{
  color: green;
  text-decoration: none;
}
a:visited{ /* visited在hover后面，这样的话hover事件就失效了 */
  color: red;
  text-decoration: none;
}
```

正确的做法是将两个事件的位置调整一下。

`a:link`：未访问时的样式，一般省略成

`a:visited`：已经访问后的样式

`a:hover`：鼠标移上去时的样式

`a:active`：鼠标按下时的样式



#### 31.怎么做移动端适配
## 写在前面
* css像素：代码中使用的逻辑像素，衡量页面上的内容大小
* 设备像素：即物理像素，控制设备显示的单位，与设备、硬件有关
* 设备独立像素：与设备无关的逻辑像素，不同于设备像素（物理像素），不是真实存在的。
* 设备像素比：定义设备像素与设备独立像素比的关系window.devicePixelRatio）设备像素比=物理像素/设备独立像素
* 分辨率：指的是屏幕上垂直和水平的总物理像素

好像面试问移动端适配还挺多的，在网上查找了很多资料（侵删），总结一下：
1. px + viewport适配
2. rem布局
  * CSS3媒体查询适配
  * 基于设计图的rem布局
  * 基于屏幕百分比的rem布局
  * 小程序的rpx布局
3. vw布局
* 通过媒体查询的方式即CSS3的meida queries
* 以天猫首页为代表的 flex 弹性布局
* 以淘宝首页为代表的 rem+viewport缩放
* rem 方式

## 一、px + viewport适配
  通过动态设置`meta`标签的`viewport`让`css`中的`1px`等于设备的`1px`。

  首先我们必须要了解到viewport是什么，viewport是用户的网页可视区域。手机浏览器就是页面放在一个虚拟的“窗口”（viewport）中，通常这个虚拟的窗口比屏幕宽，这样就不会破坏没有针对手机浏览器优化的网页的布局（不会把每个网页挤到很小的窗口中）。用户可以通过平移和缩放来看网页的不同部分。

  通常viewport是指视窗、视口，浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域。在移动端和pc端视口是不同的，pc端的视口是浏览器窗口区域，而在移动端有三个不同的视口概念：布局视口、视觉视口、理想视口

### 布局视口：
在浏览器窗口css的布局区域，布局视口的宽度限制css布局的宽。为了能在移动设备上正常显示那些为pc端浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或其他值，一般都比移动端浏览器可视区域大很多，所以就会出现浏览器出现横向滚动条的情况

### 视觉视口：
用户通过屏幕看到的页面区域，通过缩放查看显示内容的区域，在移动端缩放不会改变布局视口的宽度，当缩小的时候，屏幕覆盖的css像素变多，视觉视口变大，当放大的时候，屏幕覆盖的css像素变少，视觉视口变小。

### 理想视口：
一般来讲，这个视口其实不是真实存在的，它对设备来说是一个最理想布局视口尺寸，在用户不进行手动缩放的情况下，可以将页面理想地展示。那么所谓的理想宽度就是浏览器（屏幕）的宽度了。

设置理想视口就在header中加入这样一行代码：
```html
 <meta name="viewport"content="width=device-width,user-scalable=no,initial-scale=1.0,  maximum-scale=1.0,minimum-scale=1.0">	
 ```

## 二、rem布局
### 1.`CSS3`媒体查询适配 `meida queries`
通过查询设备的宽度来执行不同的 `css` 代码，最终达到界面的配置

```css
@media screen and (max-width: 320px){
    ....适配iphone4的css样式
}
@media screen and (max-width: 375px){
     ....适配iphone6/7/8的css样式
}
@media screen and (max-width: 414px){
    ....适配iphone6/7/8 plus的css样式
}
......
```

### 优点：

media query可以做到设备像素比的判断，方法简单，成本低，特别是对 移动和PC维护同一套代码的时候。目前像Bootstrap等框架使用这种方式 布局
方法简单，只需修改css文件
调整屏幕宽度时不用刷新页面就可以响应页面布局

### 缺点：

代码量大，不方便维护
不能够完全适配所有的屏幕尺寸，需要编写多套css样式
为了兼顾大屏幕或高清设备，会造成其他设备资源浪费，特别是加载图片资源
为了兼顾移动端和PC端各自响应式的展示效果，难免会损失各自特有的交互方式

### 2.基于设计图的rem布局
  通常我们拿到的设计图宽度的是750也就是基于iphone6/7/8的设计图，我们如果要想让1px像素等于设计图的1px该怎么做呢？

  其实很简单，直接让根元素的font-size: 0.5px即可（因为是2倍图，1px等于2实际像素，所以为0.5px）。但是市面上的机型不一定都是750px的，这个时候我们就要进行等比缩放了。

```js
html.fontSize = clientWidth / 750
```
还有一个小问题，平常开发都是基于谷歌chorme开发的，chrome并不支持font-size小于12的字体。所以可以让font-size大于12，在以上基础上将结果放大100倍，然后写样式的时候再除以100。嗯~看到这里我觉得很绕啊，不过开始放在这里了。
js伪代码：
```js
html.fontSize = clientWidth / 750 * 100
```
样式：

```css
.element {
    width: 0.1rem; /* 实际到6/7/8上就是10px */
}
```
## 3.基于屏幕百分比的rem布局
  这种方式是给元素设置百分比，例如2个div想占满宽度100%，那么一个div设置宽度为50%，这样不固定宽度，使得在不同的分辨率下都能达到适配。
  各子元素或属性的百分比设定计算：

1.子元素width、height的百分比:子元素的width或height中使用百分比，是相对于子元素的直接父元素

2.margin和padding的百分比：在垂直方向和水平方向都是相对于直接父亲元素的width，而与父元素的height无关

3.border-radius的百分比：border-radius的百分比是相对于自身宽度，与父元素无关

#### 优点：

宽度自适应，在不同的分辨率下都能达到适配

#### 缺点：

百分比的值不好计算
需要确定父级的大小，因为要根据父级的大小进行计算 各个属性中如果使用百分比，相对父元素的属性并不是唯一的
高度不好设置，一般需要固定高度

### 4. 小程序的rpx布局
  小程序有个rpx可以根据屏幕自适应。官方文档的介绍：可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素，也就是说，它内部的实现原理其实和基于设计图的rem布局的原理差不多。

  只不过小程序内部处理了一下，让rpx直接能够根据屏幕宽度自适应，而不是像rem那样依赖于根元素的font-size.

## 三、VM布局
css3中引入与视口有关的新的单位vw和vh，vw表示相对于视口的宽度，vh表示相对于视口高度。
在这里插入图片描述
那么问题来了，平时我们拿到的设计图都是基于px标记的，怎么将px转为vw呢？
vw单位换算：视口宽度为100vw占满整个视口区域，那么1vw相当于占整个视口宽度的1%，所以1px= 1/375*100 vw
所有的页面元素都可以直接进行计算换算成vw单位，但是这样计算和百分比方案计算比较类似，都会比较麻烦。

但是有一个比较厉害的插件—— postcss-px-to-viewport，可以预处理css,将px单位转换为vw单位，但是需要进行一些相关的webpack配置。详情可看上方链接官方配置解释。
```js
{
    loader: 'postcss-loader',
    options: {
    	plugins: ()=>[
        	require('autoprefixer')({
        		browsers: ['last 5 versions']
        	}),
        	require('postcss-px-to-viewport')({
        		viewportWidth: 375,
        		viewportHeight: 1334,
        		unitPrecision: 3,
        		viewportUnit: 'vw',
        		selectorBlackList: ['.ignore', '.hairlines'],
                minPixelValue: 1,
                mediaQuery: false
        	})
    	]
}
```
### 优点：

指定vw\vh相对与视口的宽高，由px换算单位成vw单位比较简单
通过postcss-px-to-viewport插件进行单位转换比较方便

### 缺点：

直接进行单位换算时百分比可能出现小数,计算不方便

兼容性- 大多数浏览器都支持、ie11不支持

少数低版本手机系统 ios8、android4.4以下不支持


### 32.浏览器兼容写法?
#### -moz-     
火狐等使用Mozilla浏览器引擎的浏览器
#### -webkit-  
Safari, 谷歌浏览器等使用Webkit引擎的浏览器
#### -o-       
Opera浏览器(早期)
#### -ms-     
Internet Explorer (不一定)

#### 为什么需要浏览器引擎前缀(Vendor Prefix)？

>这些浏览器引擎前缀(Vendor Prefix)主要是各种浏览器用来试验或测试新出现的CSS3属性特征。可以总结为以下3点：

1. 试验一些还未成为标准的的CSS属性——也许永远不会成为标准
2. 对新出现的标准的CSS3属性特征做实验性的实现
3. 对CSS3中一些新属性做等效语义的个性实现