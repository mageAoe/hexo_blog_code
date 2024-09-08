---
title: 前端面试题 - CSS篇
categories: [前端,面试题,CSS]
tags: [前端,面试题,CSS ]
date: 2024-09-08
---

前端面试题 - CSS篇

<!--more-->

## 二、CSS篇

### 如果要做优化，CSS提高性能的方法有哪些？

内联首屏关键CSS
异步加载CSS
资源压缩
合理使用选择器
减少使用昂贵的属性
不要使用@import

### 1、说一下 link 与 @import 的区别和用法？

页面导入外部css文件的方法通常有两种，一种在网页中直接link标签加入，另一种在页面中@import引入css文件。两种引入形式如下：
link引入形式：

```css
<link href="styles.css" type="text/css" />
```

@import引用形式：

```css
<style type="text/css">
    @import url("styles.css");
</style>
```

1、适用范围不同
@import可以在网页页面中使用，也可以在css文件中使用，用来将多个css文件引入到一个css文件中；而link只能将css文件引入到网页页面中。

2、功能范围不同
link属于XHTML标签，而@import是CSS提供的一种方式，link标签除了可以加载CSS外，还可以定义rel连接属性，定义RSS等，@import就只能加载CSS。
3、加载顺序不同
页面被加载的时候，link引用的CSS会同时被加载，而@import引用的CSS会等到页面全部被下载完再被加载。所以有时候浏览@import加载CSS的页面时开始会没有样式（就是闪烁）
4、兼容性
由于@import是css2.1提出的，所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题。
5、控制样式时的差别
使用link方式可以让用户切换CSS样式.现代浏览器如Firefox,Opera,Safari都支持rel=”alternate stylesheet”属性(即可在浏览器上选择不同的风格),当然你还可以使用Javascript使得IE也支持用户更换样式。
6、使用DOM控制样式时的差别
当使用JavaScript控制DOM去改变样式的时候，只能使用link标签，因为@import不是DOM可以控制的。



### 2、rgba和opacity的透明效果有什么不同？

opacity
opacity是一个属性。opacity属性的值，可以被其子元素继承，给父级div设置opacity属性，那么所有子元素都会继承这个属性，并且，该元素及其继承该属性的所有子元素的所有内容透明度都会改变。

rgba（0，0，0，0.5）
rgba是一个属性值。rgba设置的元素，只对该元素的背景色有改变，并且，该元素的后代不会继承该属性。



### 3、display:none与visibility:hidden的区别？

这两个属性都是让元素隐藏，不可见。两者区别如下：

（1）在渲染树中

display:none会让元素完全从渲染树中消失，渲染时不会占据任何空间；
visibility:hidden不会让元素从渲染树中消失，渲染的元素还会占据相应的空间，只是内容不可见。
（2）是否是继承属性

display:none是非继承属性，子孙节点会随着父节点从渲染树消失，通过修改子孙节点的属性也无法显示；
visibility:hidden是继承属性，子孙节点消失是由于继承了hidden，通过设置visibility:visible可以让子孙节点显示；
（3）修改常规文档流中元素的 display 通常会造成文档的重排，但是修改visibility属性只会造成本元素的重绘；
（4）如果使用读屏器，设置为display:none的内容不会被读取，设置为visibility:hidden的内容会被读取。

（5）display：none 隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢，就当他从来不存在。
（6）visibility：hidden 隐藏对应的元素，但是在文档布局中仍保留原来的空间。



### 4、定位布局 position中的relative、absolute、fixed、sticky它们之间的区别？

![图片](/images/2024/09/08/css-1.png)



**（1）relative:相对定位，相对于自己本身在正常文档流中的位置进行定位。**

![图片](/images/2024/09/08/css-2.png)

**（2）absolute:生成绝对定位，相对于最近一级定位不为static的父元素进行定位。**

![图片](/images/2024/09/08/css-3.png)

![图片](/images/2024/09/08/css-4.png)

**（3）fixed: 生成绝对定位，相对于浏览器窗口或者frame进行定位。（老版本IE不支持）**

![图片](/images/2024/09/08/css-5.png)

**（4）static:默认值，没有定位，元素出现在正常的文档流中。（很少用）
（5）sticky:生成粘性定位的元素，容器的位置根据正常文档流计算得出。（很少用）**



### 5、如何用CSS3画一条0.5px的直线？

```css
height: 1px;
transform: scale(0.5);
```

### 6、如何用CSS3画一个三角形？

```css
<style>
.up{
    width:0;
    height:0;
    border: 100px solid transparent;
    border-top: 100px solid red;/*红色*/
}
.down{
    width:0;
    height:0;
    border: 100px solid transparent;
    border-bottom: 100px solid blue;/*蓝色*/
}
.left{
    width:0;
    height:0;
    border: 100px solid transparent;
    border-left: 100px solid pink;/*黑色*/
}
.right{
    width:0;
    height:0;
    border: 100px solid transparent;
    border-right: 100px solid pink;/*黄色*/
}
</style>
 
<body>
    <div class="up"></div>
    <div class="down"></div>
    <div class="left"></div>
    <div class="right"></div>
</body>
```

### 7、CSS3盒子模型：标准盒模型、怪异盒模型

盒子模型分为两种:

第一种是 W3C 标准的盒子模型（标准盒模型）
第二种 IE 标准的盒子模型（怪异盒模型）
标准盒模型与怪异盒模型的表现效果的区别之处：

1、标准盒模型中 width 指的是内容区域 content 的宽度

height 指的是内容区域 content 的高度

标准盒模型下盒子的大小 = content + border + padding + margin
![图片](/images/2024/09/08/css-6.png)



2、怪异盒模型中的 width 指的是内容、边框、内边距总的宽度（content + border + padding）；height 指的是内容、边框、内边距总的高度

怪异盒模型下盒子的大小=width（content + border + padding） + margin

![图片](/images/2024/09/08/css-7.png)

除此之外，我们还可以通过属性 box-sizing 来设置盒子模型的解析模式

可以为 box-sizing 赋两个值：

1、box-sizing: content-box：默认值，border 和 padding 不算到 width 范围内，可以理解为是 W3c 的标准模型(default)。总宽=width+padding+border+margin

2、box-sizing: border-box：border 和 padding 划归到 width 范围内，可以理解为是 IE 的怪异盒模型，总宽=width+margin

- ***\*相邻块元素垂直外边距的合并\****

![图片](/images/2024/09/08/css-8.png)

- ***\*嵌套块元素垂直外边距的塌陷\****

![图片](/images/2024/09/08/css-9.png)



### 8、浮动（float)以及清除浮动的方法

(1) 脱标

![图片](/images/2024/09/08/css-10.png)



(2) 清除浮动

![图片](/images/2024/09/08/css-11.png)

![图片](/images/2024/09/08/css-12.png)

(3) 清除浮动的方法

清除浮动 —— ***\*额外标签法\****

![图片](/images/2024/09/08/css-13.png)



- 清除浮动 —— 父级添加 overflow

![图片](/images/2024/09/08/css-14.png)



- 清除浮动 —— :after 伪元素法

![图片](/images/2024/09/08/css-15.png)

- 清除浮动 —— 双伪元素清除浮动

![图片](/images/2024/09/08/css-16.png)

  

### 9、Flex布局

**1、主轴对齐方式**

使用justify-content调节元素在主轴的对齐方式

![图片](/images/2024/09/08/css-17.png)

**2、侧轴对齐方式**

使用align-items调节元素在侧轴的对齐方式

![图片](/images/2024/09/08/css-18.png)

**3、换轴**

使用flex-direction改变元素排列方向

![图片](/images/2024/09/08/css-19.png)

**4、弹性盒子换行**

目标：使用flex-wrap实现弹性盒子多行排列效果

![图片](/images/2024/09/08/css-20.png)





### 10、CSS3中“transform”属性~平面转换

**（1）位移：**transform: translate(水平移动距离, 垂直移动距离)

![图片](/images/2024/09/08/css-21.png)



（2）旋转

transform: rotate(角度); 注意：角度单位是deg

取值为正, 则顺时针旋转 Ø 取值为负, 则逆时针旋转

（3）缩放

transform: scale(x轴缩放倍数, y轴缩放倍数);
transform: scale(缩放倍数);
scale值大于1表示放大, scale值小于1表示缩小
（4）transition的基本用法

transition：[属性名] [持续时间] [速度曲线] [延迟时间]

我们可以很方便的用这个过渡来给某一个属性加上好看的动效。

例如，高度属性的值改变时，延迟 0.5 秒后以 ease 曲线进行过渡，持续2秒：

transition:height 2s ease 0.5s
或者一个属性不够，想要监听所有属性。

transition: all 2s ease .5s



### 11、CSS3中 “子绝父相” 定位布局

弄清楚这个口诀，就明白了绝对定位和相对定位的使用场景。
这个“子绝父相”太重要了，是我们学习定位的口诀，是定位中最常用的一种方式这句话的意思是：子级是绝对定位的话，父级要用相对定位。

① 子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个地方，不会影响其他的兄弟盒子。
② 父盒子需要加定位限制子盒子在父盒子内显示。
③ 父盒子布局时，需要占有位置，因此父亲只能是相对定位。
这就是子绝父相的由来，所以相对定位经常用来作为绝对定位的父级。

总结： 因为父级需要占有位置，因此是相对定位， 子盒子不需要占有位置，则是绝对定位
当然，子绝父相不是永远不变的，如果父元素不需要占有位置，子绝父绝也会遇到。



### 12、盒子居中的几种方法：“子绝父相”、“Flex布局”、“transform”

***\**\*（1）利用定位（子绝父相）、margin-left、margin-top实现\*\**\***

![图片](/images/2024/09/08/css-21.png)

（2）***\*利用定位（子绝父相）、transform属性实现\****

![图片](/images/2024/09/08/css-22.png)

（3）***\*利用flex布局实现盒子居中\****

![图片](/images/2024/09/08/css-23.png)





### 13、CSS3中有哪些新特性？

新增各种CSS选择器 （: not(.input)：所有 class 不是“input”的节点）
圆角 （border-radius:8px）
多列布局 （multi-column layout）
阴影和反射 （Shadoweflect）
文字特效 （text-shadow）
文字渲染 （Text-decoration）
线性渐变 （gradient）
旋转 （transform）
增加了旋转,缩放,定位,倾斜,动画,多背景



### 14、CSS3选择器及其优先级

![图片](/images/2024/09/08/css-24.png)



对于选择器的优先级：

标签选择器、伪元素选择器：1
类选择器、伪类选择器、属性选择器：10
id 选择器：100
内联样式：1000
注意事项：

!important声明的样式的优先级最高；
如果优先级相同，则最后出现的样式生效；
继承得到的样式的优先级最低；
通用选择器（*）、子选择器（>）和相邻同胞选择器（+）并不在这四个等级中，所以它们的权值都为 0 ；
样式表的来源不同时，优先级顺序为：内联样式 > 内部样式 > 外部样式 > 浏览器用户自定义样式 > 浏览器默认样式。



### 15、CSS3中 “transition” 过渡属性

![图片](/images/2024/09/08/css-25.png)



### 16、结构伪类选择器&伪元素选择器

1、结构伪类选择器：可方便的选取一个或多个特定的元素

:first-child 选取属于其父元素的首个子元素
:last-child 选取属于其父元素的最后一个子元素
:nth-child(n) 选择第n个子元素
　　　    n=even / 2n ：选取偶数孩子  

　　　　n=odd / 2n+1 ：选取奇数孩子

2、伪元素选择器：

::first-letter / line: 文本第一个单词 / 第一行
::selection: 改变选中文本的样式
::before & ::after
这两兄弟特性一样：1.必须要带content属性（可以为空）

　　　　　　　　　2.属于行内盒子

3、属性选择器：

div[class=xx]: 选择类名为xx的div
div[class^=xx]: 选择以类名为xx开头的div
div[class$=xx]: 选择类名是以xx结束的div
div[class*=xx]: 选择类名带有xx的div
（1）结构伪类选择器
![图片](/images/2024/09/08/css-26.png)

（2）伪元素选择器

![图片](/images/2024/09/08/css-27.png)



### 17、display的block、inline和inline-block的区别？

（1）block： 会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；

（2）inline： 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；

（3）inline-block： 将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内。

对于行内元素和块级元素，其特点如下：

行内元素

设置宽高无效；
可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
不会自动换行；
块级元素

可以设置宽高；
设置margin和padding都有效；
可以自动换行；
多个块状，默认排列从上到下。





### 18、定位堆叠顺序z-index

在使用定位布局时，可能会出现盒子重叠的情况。此时，可以使用 z-index 来控制盒子的前后次序 (z轴)

语法： 选择器 { z-index: 1; }
数值可以是正整数、负整数或 0, 默认是 auto，数值越大，盒子越靠上

如果属性值相同，则按照书写顺序，后来居上

数字后面不能加单位

只有定位的盒子才有 z-index 属性




## 三、HTML&&CSS混合篇

### 1、Localstorage、sessionStorage、cookie 的区别

共同点：都是保存在浏览器端、且同源的

三者区别：

1、cookie 数据始终在同源的 http 请求中携带（即使不需要），即 cookie 在浏览器和服务器

间来回传递，而 sessionStorage 和 localStorage 不会自动把数据发送给服务器，仅在本地保存。 cookie 数据还有路径（path）的概念，可以限制 cookie 只属于某个路径下

2、存储大小限制也不同，cookie 数据不能超过 4K，同时因为每次 http 请求都会携带 cookie、 所以 cookie 只适合保存很小的数据，如会话标识。sessionStorage 和 localStorage 虽然也有存 储大小的限制，但比 cookie 大得多，可以达到 5M 或更大。

3、数据有效期不同

sessionStorage：仅在当前浏览器窗口关闭之前有效；

localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；

cookie：只在设置的 cookie 过期时间之前有效，即使窗口关闭或浏览器关闭

4、作用域不同

sessionStorage 不在不同的浏览器窗口中共享，即使是同一个页面；

localstorage 在所有同源窗口中都是共享的；

cookie 也是在所有同源窗口中都是共享的

5、web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。

6、web Storage 的 api 接口使用更方便。



### 2、如何实现双飞翼（圣杯）布局？ 

![图片](/images/2024/09/08/css-28.png)

 1、利用定位实现两侧固定中间自适应

        父盒子设置左右 padding 值
    
        给左右盒子的 width 设置父盒子的 padding 值,然后分别定位到 padding 处.
    
        中间盒子自适应

2、利用 flex 布局实现两侧固定中间自适应

        父盒子设置 display：flex
    
        左右盒子设置固定宽高
    
        中间盒子设置 flex：1 




### 3、伪元素和伪类的区别和作用？

- 伪元素：在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。它们只在外部显示可见，但不会在文档的源代码中找到它们，因此，称为“伪”元素。例如：

- ```css
  p::before {content:"第一章：";}
  p::after {content:"Hot!";}
  p::first-line {background:red;}
  p::first-letter {font-size:30px;}
  ```

- 伪类：将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。例如：

- ```css
  a:hover {color: #FF00FF}
  p:first-child {color: red}
  ```



**总结：** 伪类是通过在元素选择器上加⼊伪类改变元素状态，⽽伪元素通过对元素的操作进⾏对元素的改变。



### 4、img 的 alt 与 title 的异同，还有实现图片懒加载的原理？

异同
alt 是图片加载失败时，显示在网页上的替代文字； title 是鼠标放上面时显示的文字,title

是对图片的描述与进一步说明;

这些都是表面上的区别，alt 是 img 必要的属性，而 title 不是 对于网站 seo 优化来说，title 与 alt 还有最重要的一点： 搜索引擎对图片意思的判断，主 要靠 alt 属性。所以在图片 alt 属性中以简要文字说明，同时包含关键词，也是页面优化的 一部分。条件允许的话，可以在 title 属性里，进一步对图片说明 由于过多的图片会严重影响网页的加载速度，并且移动网络下的流量消耗巨大，所以 说延迟加载几乎是标配了。

原理
图片懒加载的原理很简单，就是我们先设置图片的 data-set 属性（当然也可以是其他任意的， 只要不会发送 http 请求就行了，作用就是为了存取值）值为其图片路径，由于不是 src，所 以不会发送 http 请求。 然后我们计算出页面 scrollTop 的高度和浏览器的高度之和， 如果 图片举例页面顶端的坐标 Y（相对于整个页面，而不是浏览器窗口）小于前两者之和，就说明图片就要显示出来了（合适的时机，当然也可以是 其他情况），这时候我们再将data-set 属性替换为 src 属性即可。



### 5、BFC 是什么？ 

定义
BFC (Block formatting context) 直译为 "块级格式化上下文"。它是一个独立的渲染区域，

只有 Block-level box 参与，它规定了内部的 Block-level Box 如何布局，并且与这个区域外部毫不相干。

布局规则

1、内部的 Box 会在垂直方向，一个接一个地放置
2、Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin
会发生重叠
3、每个元素的 margin box 的左边， 与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
4、BFC 的区域不会与 float box 重叠
5、BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此
6、计算 BFC 的高度时，浮动元素也参与计算
哪些元素会生成 BFC

1、根元素
2、float 属性不为 none
3、position 为 absolute 或 fixed
4、display 为 inline-block， table-cell， table-caption， flex， inline-flex
5、overflow 不为 visible