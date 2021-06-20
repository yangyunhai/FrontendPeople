### 1.CSS3边框：
* border-radius：CSS3圆角边框。
* box-shadow：CSS3边框阴影。
* border-image：CSS3边框图片。
### 2.CSS3背景：
* background-size： 属性规定背景图片的尺寸。
* background-origin ：属性规定背景图片的定位区域。
* background-clip:指定背景图片从什么位置开始裁剪
### 3.CSS3文字效果：
* text-shadow：在 CSS3 中，text-shadow 可向文本应用阴影。
* word-wrap :单词太长的话就可能无法超出某个区域，允许对长单词进行拆分，并换行到下一行
### 4.CSS3 2D转换：
* transform：通过 CSS3 转换，我们能够对元素进行移动、缩放、转动、拉长或拉伸。
* translate()：元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 
```css
transform：translate（50px,100px）
```
* rotate()：实现元素的旋转效果
```css
transform:rotate(7deg);
```
* scale():实现元素的缩放效果
```css
transform:scale(1.1,1.1);
```
* skew():实现元素的倾斜效果
```css
transform:skew(10deg,10deg);
```
* matrix()： 方法需要六个参数，包含数学函数，允许您：旋转、缩放、移动以及倾斜元素。
```css
transform: matrix(3, 0, 0, 0.5, 0, 0);
``` 
clip-path:clip-path里面的polygon功能，我们可以通过它来绘制多边形

```css
-webkit-clip-path: polygon(0 100%, 50% 0, 100% 100%);
```
### 5.CSS3 过渡
transition
```css
transition: width 2s;
```
### 6.CSS3 动画
animation
```css
animation:myAnimation 5s infinite;
```
### 选择器
* div:nth-last-child(n) 
* div:nth-of-type(n) 
* div:nth-last-of-type(n) 
* div:last-child 
* div:first-of-type 
* div:only-child 
* div:only-of-type 
* div:empty 
* div:checked 
* div:enabled 
* div:disabled 
* div::selection 
* div:not(s)