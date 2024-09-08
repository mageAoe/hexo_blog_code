---
title: 前端面试题 - Javascript篇
categories: [前端,面试题,Javascript]
tags: [前端,面试题,Javascript ]
date: 2024-09-08
---

前端面试题 - Javascript篇

<!--more-->

## 三、JavaScript篇

### js 判断数据类型的几种方法

```js
typeof str     // "string" 字符串
typeof num     // "number" 数值
typeof array   // "object" 对象（可以和函数区别开）
// 👆注意，数组也是一个对象
typeof date    // "object" 对象 
typeof func    // "function" 函数
typeof symbol  // "symbol"
```

引申： 如果要判断 array 跟 object类型呢

```js
Object.prototype.toString.call(arr) // '[object Array]'
```





### 防抖和节流，应用场景

防抖和节流都是防止某一时间频繁触发，但是原理却不一样。
防抖是将多次执行变为只执行一次，节流是将多次执行变为每隔一段时间执行。
防抖(debounce)：
search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
节流(throttle)：
鼠标不断点击触发，mousedown(单位时间内只触发一次)
监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

### Promise.all和Promise.race的区别，应用场景

Promise.all()可以将多个实例组装个成一个新实例，成功的时候返回一个成功的数组；失败的时候则返回最先被reject失败状态的值。
适用场景：比如当一个页面需要在很多个模块的数据都返回回来时才正常显示，否则loading。

promise.all中的子任务是并发执行的，适用于前后没有依赖关系的。Promise.race()意为赛跑的意思，也就是数组中的任务哪个获取的块，就返回哪个，不管结果本身是成功还是失败。一般用于和定时器绑定，比如将一个请求和三秒的定时器包装成Promise实例，加入到Promise队列中，请求三秒中还没有回应时，给用户一些提示或相应的操作。

### 微任务和宏任务的区别

1.宏任务：当前调用栈中执行的代码成为宏任务。（主代码快，定时器等等）。
2.微任务： 当前（此次事件循环中）宏任务执行完，在下一个宏任务开始之前需要执行的任务,可以理解为回调事件。（promise.then，proness.nextTick等等）。

3. 宏任务中的事件放在callback queue中，由事件触发线程维护；微任务的事件放在微任务队列中，由js引擎线程维护。
   微任务：process.nextTick、MutationObserver、Promise.then catch finally
   宏任务：I/O、setTimeout、setInterval、setImmediate、requestAnimationFrame



### 1、JS基础类型和复杂类型

JS数据基础类型有：
String、Number、Boolean、Null、undefined五种基本数据类型，加上新增的两种ES6的类型Symbol、BigInt

JS有三种 复杂类型 （引用数据类型）： 
Object（对象）、Array（数组）、function（函数）



### 2、箭头函数与普通函数的区别？

(1) 箭头函数比普通函数更加简洁
        如果没有参数 , 就直接写一个空括号即可 , 如果只有一个参数 , 可以省去参数的括号     如果有多个参数 , 用逗号分割 , 如果函数体的返回值只有一句 , 可以省略大括号。
(2) 箭头函数没有自己的this
        箭头函数不会创建自己的this, 所以它没有自己的this, 它只会在自己作用域的上一层继承this。所以箭头函数中this的指向在它在定义时已经确定了, 之后不会改变。
(3) 箭头函数继承来的this指向永远不会改变
(4) call()、apply()、bind()等方法不能改变箭头函数中this的指向  
(5) 箭头函数不能作为构造函数使用
        由于箭头函数时没有自己的this，且this指向外层的执行环境，且不能改变指向，所以不能当做构造函数使用。 
(6) 箭头函数没有自己的arguments对象。在箭头函数中访问arguments实际上获得的是它外层函数的arguments值。 
(7) 箭头函数没有prototype 
(8) 补充：箭头函数的this指向哪⾥？
        箭头函数不同于传统JavaScript中的函数，箭头函数并没有属于⾃⼰的this，它所谓的this是捕获其所在上下⽂的 this 值，作为⾃⼰的 this 值，并且由于没有属于⾃⼰的this，所以是不会被new调⽤的，这个所谓的this也不会被改变。



### 3、JS中null和undefined的判断方法和区别？

1.undefined 的判断　
(1) undefined表示缺少值，即此处应该有值，但是还没有定义
(2) 变量被声明了还没有赋值，就为undefined
(3) 调用函数时应该提供的参数还没有提供，该参数就等于undefined
(4) 对象没有赋值的属性，该属性的值就等于undefined
(5) 函数没有返回值，默认返回undefined
2.null 的判断　
(1) null表示一个值被定义了，但是这个值是空值
(2) 作为函数的参数，表示函数的参数不是对象
(3) 作为对象原型链的终点 （Object.getPrototypeOf(Object.prototype)）
(4) 定义一个值为null是合理的，但定义为undefined不合理（var name = null）
3.typeof 类型不同　　

```js
typeof null; // 'object'
typeof undefined; // 'undefined'
```

***\**\*4.Number() 转数字也不同\*\**\***

```js
Number(null); // 0
Number(undefined); // NaN 
```





### 4、原型链

前提须知：
(1) prototype：所有的函数都有原型prototype属性，这个属性指向函数的原型对象。
(2) __proto__，这是每个对象(除null外)都会有的属性，叫做__proto__，这个属性会指向该对象的原型。
(3) constructor: 每个原型都有一个constructor属性，指向该关联的构造函数。

原型链：获取对象时，如果这个对象上本身没有这个属性时，它就会去它的原型__proto__上去找，如果还找不到，就去原型的原型上去找...一直直到找到最顶层（Object.prototype）为止，Object.prototype对象也有__proto__属性，值为null
此外，每一个prototype原型上都会有一个constructor属性，指向它关联的构造函数。

![图片](/images/2024/09/08/js-1.png)



### 5、v-show 与 v-if 的区别？

- ***\*`v-show`\****指令是通过修改元素的`display`的`CSS`属性让其显示或者隐藏；
- ***\*`v-if`\****指令是直接 **销毁** 和 **重建** `DOM`达到让元素显示和隐藏的效果；
- 使用 `v-show`会更加节省性能上的开销；当只需要一次显示或隐藏时，使用`v-if`更加合理。



### 6、keep-alive 的作用是什么?

官网解释：包裹动态组件时，会缓存不活动的组件实例，主要用于保留组件状态或避免重新渲染。
作用：实现组件缓存，保持这些组件的状态，以避免反复渲染导致的性能问题。 需要缓存组件 频繁切换，不需要重复渲染。
场景：tabs标签页 后台导航，vue性能优化。
原理：Vue.js内部将DOM节点抽象成了一个个的VNode节点，keep-alive组件的缓存也是基于VNode节点的而不是直接存储DOM结构。它将满足条件（pruneCache与pruneCache）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点从cache对象中取出并渲染。



### 7、闭包的理解?

**概念**：有权访问另一个函数内部变量的函数。
**本质**：是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。
**面试：什么是闭包？**
    通俗的来说：闭包是在一个函数内部在定一个函数，然后内部函数访问外部函数的一个变量就会形成闭包，闭包的话会形成一个私有空间，然后避免全局变量的一个污染，然后会持久化存储数据到内存中，但是闭包也有弊端，它会导致内存泄漏

**拓展：内存泄漏怎么解决？**
首先避免它的使用，其次的话就是变量执行完以后，可以让它赋值为null，最后利用JS的一个垃圾回收机制进行回收

**闭包用处**：
读取内部函数的变量；
这些变量的值始终会保持在内存中，不会在外层函数调用后被自动清除
**闭包优点：**
变量会一直在内存中；        
避免全局变量的污染；
私有变量的存在；
**闭包缺点：**
    变量长期储存在内存中，会增大内存的使用量，使用不当会造成内存泄露

**判断闭包的3个特点：**
　　1.函数嵌套函数；

　　2.内部函数一定操作了外部函数的局部变量；

　　3.外部函数一定将内部函数返回到外部并保存在一个全局变量中；

**判断闭包的执行结果：**
　　1.外部函数被调用了几次就有几个受保护的局部变量的副本；

　　2.来自一个闭包的函数被调用几次，受保护的局部变量就变化几次；

**闭包特性：**
函数嵌套函数；
内部函数可以直接使用外部函数的局部变量；
变量或参数不会被垃圾回收机制回收；





### 8、JS垃圾回收机制

垃圾回收机制（Garbage Collection）简称GC：是JavaScript中使用的内存管理系统的基本组成部分
JavaScript 是在创建变量（对象，字符串等）时自动进行了分配内存，并且在不使用它们时会 “自动” 释放、释放的过程成为垃圾回收
内存在不使用的时候会被垃圾回收器自动回收



### 9、nextTick的实现？

nextTick是Vue提供的一个全局API,是在下次DOM更新循环结束之后执行延迟回调，在修改数据之后使用$nextTick，则可以在回调中获取更新后的DOM。
Vue在更新DOM时是异步执行的。只要侦听到数据变化，Vue将开启1个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个watcher被多次触发，只会被推入到队列中-次。这种在缓冲时去除重复数据对于避免不必要的计算和DOM操作是非常重要的。nextTick方法会在队列中加入一个回调函数，确保该函数在前面的dom操作完成后才调用
比如，我在干什么的时候就会使用nextTick，传一个回调函数进去，在里面执行dom操作即可。简单了解nextTick的实现，它会在callbacks里面加入我们传入的函数，然后用timerFunc异步方式调用它们，首选的异步方式会是Promise。这让我明白了为什么可以在nextTick中看到dom操作结果。
**实现原理**：在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后立即使用 nextTick 来获取更新后的 DOM。 nextTick主要使用了宏任务和微任务。 根据执行环境分别尝试采用Promise、MutationObserver、setImmediate，如果以上都不行则采用setTimeout定义了一个异步方法，多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列。

### 10、混入mixin的原理？

mixin 项目变得复杂的时候，多个组件间有重复的逻辑就会用到mixin
多个组件有相同的逻辑，抽离出来，其实mixin并不是完美的解决方案，会存在一些问题
如：vue3提出的Composition API旨在解决这些问题【追求完美是要消耗一定的成本的，如开发成本】
场景：PC端新闻列表和详情页一样的右侧栏目，可以使用mixin进行混合
劣势：

1. 变量来源不明确，不利于阅读
2. 多mixin可能会造成命名冲突
3. mixin和组件可能出现多对多的关系，使得项目复杂度变高




### 11、列举和数组操作相关的方法

![图片](/images/2024/09/08/js-2.png)

![图片](/images/2024/09/08/js-3.png)

### 12、typeof和instanceof的区别是什么？

（1）typeof 的返回值是一个字符串，用来说明变量的数据类型；

typeof用于数据类型的判断，返回值有number、string、boolean、function、undefined、object 六个。但是，在其中你会发现，typeof判断null、array、object以及函数的实例（new + 函数）时，它返回的都是object。这就导致在判断这些数据类型的时候得不到真实的数据类型。所以，typeof 存在的弊端——它虽然可以判断基本数据类型（null 除外），但是引用数据类型中，除了function 类型以外，其他的也无法判断。

（2）instanceof的返回值是布尔值，用于判断一个变量是否属于某个对象的实例。instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型。

###  

### 13、JS中 “==“和“===“的区别详解

区别

===：三个等号我们称为等同符，当等号两边的值为相同类型的时候，直接比较等号两边 的值，值相同则返回 true，若等号两边的值类型不同时直接返回 false。也就是说三个等号既要判断值也要判断类型是否相等。
==：两个等号我们称为等值符，当等号两边的值为相同类型时比较值是否相同，类型不同时会发生类型的自动转换，转换为相同的类型后再作比较。也就是说两个等号只要值相等就可以。



### 14、如何用原生 JS给一个按钮绑定两个 onclick 事件？

```js
<button id="btn">点击</button>
 
<script>
    //通过事件监听  绑定多个事件
    let btn = document.getElementById('btn')
    btn.addEventListener('click', one)
    btn.addEventListener('click', two)
    function one() {
        alert('第一个')
    }，
    function two() {
        alert('第二个')
    }，
</script>
```





### 15、var、let和const的区别？



| 区别               | var  | let  | const |
| ------------------ | ---- | ---- | ----- |
| 块级作用域         | ❌    | ✔️    | ✔️     |
| 是否存在变量提升   | ✔️    | ❌    | ❌     |
| 是否添加全局属性   | ✔️    | ❌    | ❌     |
| 重复声明同名变量   | ✔️    | ❌    | ❌     |
| 是否存在暂时性死区 | ❌    | ✔️    | ✔️     |
| 是否必须设置初始值 | ❌    | ❌    | ✔️     |
| 能否改变指针方向   | ✔️    | ✔️    | ❌     |

（1）块级作用域：  块作用域由 { }包括，let和const具有块级作用域，var不存在块级作用域。块级作用域解决了 ES5 中的两个问题：
内层变量可能覆盖外层变量
用来计数的循环变量泄露为全局变量
（2）变量提升： var存在变量提升，let和const不存在变量提升，即在变量只能在声明之后使用，否在会报错。

（3）给全局添加属性： 浏览器的全局对象是window，Node的全局对象是global。var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。

（4）重复声明： var声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的遍历。const和let不允许重复声明变量。

（5）暂时性死区： 在使用let、const命令声明变量之前，该变量都是不可用的。这在语法上，称为暂时性死区。使用var声明的变量不存在暂时性死区。

（6）初始值设置： 在变量声明时，var 和 let 可以不用设置初始值。而const声明变量必须设置初始值。

（7）指针指向： let和const都是ES6新增的用于创建变量的语法。 let创建的变量是可以更改指针指向（可以重新赋值）。但const声明的变量是不允许改变指针的指向。




### 16、讲解js的call、apply和bind区别？

首先，call apply bind三个方法都可以用来改变函数的this指向，具体区别如下：

call( ) 是接收一个及其以上的参数，第一个参数表示this要指向的对象，其余参数表示调用函数需要传入的参数，返回调用函数的返回结果，属于立即执行函数；
apply( ) 是接收两个参数，第一个参数表示this要指向的对象，第二参数表示调用函数需要传入的参数所组成的数组，返回调用函数的返回结果，属于立即执行函数；
bind( ) 是接收一个及其以上的参数，和call(）一致，但是其返回是一个函数，而不是调用函数的返回结果
call、apply、bind相同点：都是改变this的指向，传入的第一个参数都是绑定this的指向，在非严格模式中，如果第一个参数是nul或者undefined，会把全局对象（浏览器是window）作为this的值，要注意的是，在严格模式中，null 就是 null，undefined 就是 undefined
call和apply唯一的区别是：call传入的是参数列表，apply传入的是数组，也可以是类数组
bind和call、apply的区别： bind返回的是一个改变了this指向的函数，便于稍后调用，不像call和apply会立即调用；bind和call很像，传入的也是参数列表，但是可以多次传入，不需要像call，一次传入
值得注意：当 bind 返回的函数 使用new作为构造函数时，绑定的 this 值会失效，this指向实例对象，但传入的参数依然生效 （new调用的优先级 > bind调用）



### 17、谈谈你对webpack的理解？

![图片](/images/2024/09/08/js-4.png)



1、谈谈你对Webpack的理解

Webpack是一个模块打包工具，可以使用它管理项目中的模块依赖，并编译输出模块所需的静态文件。
它可以很好地管理、打包开发中所用到的HTML,CSS,JavaScript和静态文件（图片，字体）等，让开发更高效。
对于不同类型的依赖，Webpack有对应的模块加载器，而且会分析模块间的依赖关系，最后合并生成优化的静态资源。
2、Webpack的基本功能有哪些？

代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等等
文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等
代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
模块合并：在采用模块化的项目有很多模块和文件，需要构建功能把模块分类合并成一个文件
自动刷新：监听本地源代码的变化，自动构建，刷新浏览器
代码校验：在代码被提交到仓库前需要检测代码是否符合规范，以及单元测试是否通过
自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。
3、Webpack构建过程？

从entry里配置的module开始递归解析entry依赖的所有module
每找到一个module，就会根据配置的loader去找对应的转换规则
对module进行转换后，再解析出当前module依赖的module
这些模块会以entry为单位分组，一个entry和其所有依赖的module被分到一个组Chunk
最后Webpack会把所有Chunk转换成文件输出在整个流程中Webpack会在恰当的时机执行plugin里定义的逻辑
4、有哪些常见的Loader?

optimize-css-assets-plugin：压缩css；
file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
url-loader：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
json-loader: 加载 JSON 文件（默认包含）
ts-loader: babel-loader：把 ES6 转换成 ES5
ts-loader: 将 TypeScript 转换成 JavaScript
less-loader：将less代码转换成CSS
eslint-loader：通过 ESLint 检查 JavaScript 代码
vue-loader:加载 Vue单文件组件
5、有哪些常见的Plugin？

html-webpack-plugin：自动创建一个html文件，并把打包好的js插入到html中
uglifyjs-webpack-plugin：不支持 ES6 压缩 ( Webpack4 以前)
mini-css-extract-plugin: 在每一次打包之前，删除整个输出文件夹下的内容；
clean-webpack-plugin: 抽离css代码放到一个单独的文件中；
copy-webpack-plugin: 拷贝文件
webpack-bundle-analyzer: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)
optimize-css-assets-plugin：压缩css；
6、那你再说一说Loader和Plugin的区别？

Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。
Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。
Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。
7、如何优化 Webpack 的构建速度？

（1）使用高版本的 Webpack 和 Node.js

（2）压缩代码

通过 uglifyjs-webpack-plugin 压缩JS代码
通过 mini-css-extract-plugin 提取 chunk 中的 CSS 代码到单独文件，通过 css-loader 的 minimize 选项开启 cssnano 压缩 CSS。
（3）多线程/多进程构建：thread-loader, HappyPack

（4）压缩图片: image-webpack-loader

（5）缩小打包作用域

exclude/include (确定 loader 规则范围)
resolve.modules 指明第三方模块的绝对路径 (减少不必要的查找)
resolve.mainFields 只采用 main 字段作为入口文件描述字段 (减少搜索步骤，需要考虑到所有运行时依赖的第三方模块的入口文件描述字段)
resolve.extensions 尽可能减少后缀尝试的可能性
noParse 对完全不需要解析的库进行忽略 (不去解析但仍会打包到 bundle 中，注意被忽略掉的文件里不应该包含 import、require、define 等模块化语句)
ignorePlugin (完全排除模块)
8、说一下 Webpack 的热更新原理吧？

Webpack 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。
HMR的核心就是客户端从服务端拉去更新后的文件，准确地说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个 Websocket，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 jsonp 请求获取该chunk的增量更新。
后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 HotModulePlugin 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像react-hot-loader 和 vue-loader 都是借助这些 API 实现 HMR。
9、什么是bundle？什么是chunk？什么是module？

bundle：是webpack打包后的一个文件；
chunk：代码块，一个chunk 可能有很多的模块组成，用于合并和分割代码；
module：模块，在webpack中，一切都是模块，一个文件就是一个模块，她从入口开始查找webpack依赖的所有模块
10、webpack和grunt以及gulp有什么不同？

grunt和gulp是基于任务处理的工具，我们需要把我们要做的事分配成各种各样的任务，grunt和gulp会自动执行各种分配的任务，像流水线一样，把资源放上去通过不同的插件进行加工，他的插件非常丰富，能够为我们打造各种工作流；
webpack是模块化打包工具，把所有文件都当作模块进行处理，也就是说webpack和grunt和gulp是两种完全不一样的东西；

### 18、 const定义的对象属性是否可以改变？

情况一：const定义的变量存在块级作用域，且不存在变量提升，一般用于定义常量，定义的时候必须初始化。

答：不可以，const定义的如果是基本数据类型（string，number，boolean，null，undifined，symbol），定义后就不可再修改，如果修改，会报错。
![图片](/images/2024/09/08/js-5.png)



情况二：那么如果是const定义的对象呢？是否可以修改对象中的属性？
答案：可以

原因：对象是引用类型的，const定义的对象t中保存的是指向对象t的指针，这里的“不变”指的是指向对象的指针不变，而修改对象中的属性并不会让指向对象的指针发生变化，所以用const定义对象，对象的属性是可以改变的。
![图片](/images/2024/09/08/js-6.jpeg)



### 19、栈溢出及解决方法？

栈溢出(stack Overflow)

缓冲区溢出是由于C语言系列设有内置检查机制来确保复制到缓冲区的数据不得大于缓冲区的大小，因此当这个数据足够大的时候，将会溢出缓冲区的范围。
栈溢出就是缓冲区溢出的一种。 由于缓冲区溢出而使得有用的存储单元被改写, 往往会引发不可预料的后果。程序在运行过程中，为了临时存取数据的需要，一般都要分配一些内存空间，通常称这些空间为缓冲区。如果向缓冲区中写入超过其本身长度的数据，以致于缓冲区无法容纳，就会造成缓冲区以外的存储单元被改写，这种现象就称为缓冲区溢出。缓冲区长度一般与用户自己定义的缓冲变量的类型有关。

由于缓冲区溢出而使得有用的存储单元被改写，往往会引发不可预料的后果。向这些单元写入任意的数据，一般只会导致程序崩溃之类的事故，对这种情况我们也至多说这个程序有Bug。但如果向这些单元写入的是精心准备好的数据，就可能使得程序流程被劫持，致使不希望的代码被执行，落入攻击者的掌控之中，这就不仅仅是bug，而是漏洞(exploit)了。

栈溢出的解决方法

减少栈空间的需求，不要定义占用内存较多的auto变量，应该将此类变量修改成指针，从堆空间分配内存。
函数参数中不要传递大型结构/联合/对象，应该使用引用或指针作为函数参数。
减少函数调用层次，慎用递归函数，例如A->B->C->A环式调用。

### 20、JS如何实现多线程？

什么是JavaScript的多线程？
JavaScript本身是单线程的，但是可以通过实现多线程来提高性能和用户体验。多线程允许JavaScript在等待用户交互或网络请求时，执行其他任务，从而提高页面加载速度和响应速度。

JavaScript中有哪些实现多线程的方式？
JavaScript有多种实现多线程的方式，包括Web Workers、SharedArrayBuffer、WebAssembly等。其中，Web Workers允许在后台线程中运行JavaScript代码，而SharedArrayBuffer和BufferSource API则允许在多个线程之间共享数据。

如何使用Web Workers实现多线程？
使用Web Workers实现多线程需要创建一个新的worker线程，并将需要执行的代码作为字符串传递给worker。worker线程可以访问全局对象messageChannel的postMessage方法来发送消息，主线程可以使用onmessage方法来接收消息并执行相应的操作。

如何保证多线程安全？
多线程环境下的安全问题主要包括数据竞争和死锁等。为了解决这些问题，需要使用同步机制，如使用Promise、async/await等异步编程方式，或者使用事件循环、共享内存等机制来保证数据的一致性和安全性。

描述一个实际的多线程应用场景。
在实际应用中，多线程可以用于提高页面加载速度和响应速度，例如在电商网站中，可以使用Web Workers在后台线程中加载和处理商品图片，从而提高页面加载速度和用户体验。同时，多个并发请求也可以使用Web Workers并行处理，提高系统性能和响应速度。

### 21、浅拷贝和深拷贝区别概念常见情况？

- **浅拷贝：**在栈内存中重新开辟一块内存空间，并将拷贝对象储存在栈内存中的数据存放到其中。
- **深拷贝：**基于浅拷贝的过程如果栈内存里面存储的是一个地址，那么其在堆内存空间中的数据也会被重新拷贝一个并由栈空间新创建的地址指向。

![图片](/images/2024/09/08/js-7.png)

1、基本类没有问题
因为，基本类型赋值时，赋的是数据（所以，不存在深拷贝和浅拷贝的问题）。

 例如1：
    var x = 100;
    var y = x; //此时x和y都是100;
   如果要改变y的值，x的值不会改变。

2、引用类型有问题
因为，引用类型赋值时，赋的值地址（就是引用类型变量在内存中保存的内容）
 例如2：
var arr1 = new Array(12,23,34)
var arr2 = arr1;  //这就是一个最简单的浅拷贝
如果要改变arr2所引用的数据：arr2[0]=100时，那么arr1[0]的值也是100。
原因就是 arr1和arr2引用了同一块内存区域（以上的第二点中有体现）。
这是最简单的浅拷贝，因为，只是把arr1的地址拷贝的一份给了arr2，并没有把arr1的数据拷贝一份。所以，拷贝的深度不够。

一、常见的 “浅” 拷贝方法：

除了上面我们演示的对于赋值操作，下面将介绍一些开发中可能会用到，当然也可以会被面试官问到的实现深浅拷贝的方法。

1. Object.assign()
   方法解释：方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象,它将返回目标对象可以实现一个浅拷贝的效果。
   参数一：目标对象
   参数二：源对象

   ```js
   var obj1 = {
               a: 1,
               b: 2,
               c: ['c', 't', 'r']
           }
   var obj2 = Object.assign({}, obj1);
   obj2.c[1] = 5;
   obj2.b = 3
   console.log(obj1); // {a:1,b:2,c:["c", 5, "r"]}
   console.log(obj2); // {a:1,b:3,c:["c", 5, "r"]}
   console.log(obj1.c); // ["c", 5, "r"]
   console.log(obj2.c); // ["c", 5, "r"]
   ```

注意：可见Object.assign()方法对于一维数据是深拷贝效果，但是对于多维数据是浅拷贝效果。Object.assign是一个浅拷贝,它只是在根属性(对象的第一层级)创建了一个新的对象，但是对于属性的值是仍是对象的话依然是浅拷贝，

2. slice()

方法解释：数组进行截取，如果不传参数,会使用默认值,得到一个与原数组元素相同的新数组。
参数一：截取的起始位置
参数二：截取的结束位置

```js
var a = [1, [1, 2], 3, 4];
var b = a.slice();
a[0] = 99
b[1][0] = 2;
console.log(a); // [99,[2,2],3,4]
console.log(b); // [1,[2,2],3,4]
```

注意：可见slice()方法也只是对一维数据进行深拷贝，但是对于多维的数据还是浅拷贝效果。

**3. concat()方法**

方法解释：数组的拼接(将多个数组或元素拼接形成一个新的数组)，不改变原数组，如果不传参数,会使用默认值，得到一个与原数组元素相同的新数组 (复制数组)。

```js
var a = [1, 2, [3, 4]]
var c = [];
var b = c.concat(a);
a[0] = 99
b[2][1] = 88
console.log(a); // [99,2,[3,88]]
console.log(b); // [1,2,[3,88]]
```

注意：可见concat()方法也只对一维数据具有深拷贝效果，对于多维的数据任然只是浅拷贝

4. ES6拓展运算符

```js
var a = [1, 2, [3, 4]]
var b = [...a];
a[2][1] = 88
b[1] = 99
console.log(a); // [1,2,[3,88]]
console.log(b); // [1,99,[3,88]]
```

注意: 可见ES6的展开运算符对于一维数据是深拷贝效果，但是对于多维数据任然是浅拷贝效果。

二、实现 “深” 拷贝常见方法：

1. JSON.parse(JSON.stringify(obj))

JSON.stringify()是目前前端开发过程中最常用的深拷贝方式，原理是把一个对象序列化成为一个JSON字符串，将对象的内容转换成字符串的形式再保存在磁盘上，再用JSON.parse()反序列化将JSON字符串变成一个新的对象

JSON.stringfy() 将对象序列化成json对象
JSON.parse() 反序列化——将json对象反序列化成js对象

```js
function deepCopy(obj1){
    let _obj = JSON.stringify(obj1);
    let obj2 = JSON.parse(_obj);
    return obj2;
}
var a = [1, [1, 2], 3, 4];
var b = deepCopy(a);
b[1][0] = 2;
console.log(a); // 1,1,2,3,4
console.log(b); // 1,2,2,3,4
```

注意：它会抛弃对象的constructor，深拷贝之后，不管这个对象原来的构造函数是什么，在深拷贝之后都会变成Object类型，这种方法能正确处理的对象只有 Number, String, Boolean, Array, 扁平对象，
也就是说，只有可以转成JSON格式的对象才可以这样用，像function没办法转成JSON。

2. 使用第三方库实现对象的深拷贝，比如：lodash、jQuery

   ```js
   import lodash from 'lodash'
   var objects = [1,{ 'a': 1 }, { 'b': 2 }]; 
   var deep = lodash.cloneDeep(objects);
   deep[0] = 2;
   deep[1].a = 2;
   console.log(objects); // [1,{ 'a': 1 }, { 'b': 2 }]
   console.log(deep); //[2,{ 'a': 2 }, { 'b': 2 }]
   ```

**3. 递归**

这里简单封装了一个deepClone的函数，for in遍历传入参数的值，如果值是引用类型则再次调用deepClone函数，并且传入第一次调用deepClone参数的值作为第二次调用deepClone的参数，如果不是引用类型就直接复制

```js
var obj1 = {
    a:{
        b:1
    }
};
function deepClone(obj) {
    var cloneObj = {}; //在堆内存中新建一个对象
    for(var key in obj){ //遍历参数的键
       if(typeof obj[key] ==='object'){ 
          cloneObj[key] = deepClone(obj[key]) //值是对象就再次调用函数
       }else{
           cloneObj[key] = obj[key] //基本类型直接复制值
       }
    }
    return cloneObj 
}
var obj2 = deepClone(obj1);
obj1.a.b = 2;
console.log(obj2); //{a:{b:1}}
```

但是还有很多问题

- 首先这个deepClone函数并不能复制不可枚举的属性以及Symbol类型
- 这里只是针对Object引用类型的值做的循环迭代，而对于Array,Date,RegExp,Error,Function引用类型无法正确拷贝
- 对象循环引用成环了的情况

### 22、事件循环，Promise和async/await的详解

事件循环event loop它的执行顺序：

一开始整个脚本作为一个宏任务执行
执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
执行浏览器UI线程的渲染工作
检查是否有Web Worker任务，有则执行
执行完本轮的宏任务，回到2，依此循环，直到宏任务和微任务队列都为空
微任务包括：MutationObserver、Promise.then()或catch()、Promise为基础开发的其它技术，比如fetch API、V8的垃圾回收过程、Node独有的process.nextTick。

宏任务包括：script 、setTimeout、setInterval 、setImmediate 、I/O 、UI rendering。

注意⚠️：在所有任务开始的时候，由于宏任务中包括了script，所以浏览器会先执行一个宏任务，在这个过程中你看到的延迟任务(例如setTimeout)将被放到下一轮宏任务中来执行。

Promise和async/await是JavaScript中处理异步操作的两种方式。

Promise是一种用于处理异步操作的对象。它可以表示一个异步操作的最终完成或失败，并返回相应的结果或错误信息。Promise有三种状态：pending（进行中）、fulfilled（已完成）和rejected（已拒绝）。通过调用Promise的then()方法可以注册回调函数来处理异步操作的结果。
async/await是ES8引入的一种更加简洁的处理异步操作的方式。async函数是一个返回Promise对象的函数，其中可以使用await关键字来等待一个Promise对象的解决。await关键字可以暂停async函数的执行，直到Promise对象解决为止，并返回解决后的结果。
区别：

- 语法上，Promise使用then()和catch()方法来处理异步操作的结果，而async/await使用async函数和await关键字来等待异步操作的结果。
- 可读性上，async/await更加直观和易于理解，代码结构更加清晰，而Promise则需要通过链式调用then()方法来处理多个异步操作。
- 错误处理上，Promise使用catch()方法来捕获错误，而async/await可以使用try-catch语句来捕获错误。
  详细解答：

JavaScript的异步机制包括以下几个步骤：

```
（1）所有同步任务都在主线程上执行，行成一个执行栈
（2）主线程之外，还存在一个任务队列，只要异步任务有了结果，就会在任务队列中放置一个事件
（3）一旦执行栈中的所有同步任务执行完毕，系统就会读取任务队列，看看里面还有哪些事件，哪些对应的异步任务，于是异步任务结束等待状态，进入执行栈，开始执行
（4）主线程不断的重复上面的第三步
```

1. promise的用法

Promise,简单来说就是一个容器，里面保存着某个未来才会结束的时间(通常是一个异步操作的结果)

- **基本语法**

```js
 let obj = new Promise((resolve,reject) => {
        //...
        resolve('success')
    });
    
 obj.then(result => {
        console.log(result); //success
 });
```

promise共有三个状态

pending（执行中）、resolve（成功）、rejected（失败）

链式调用
Promise 链式调用是一种编程模式，允许在异步操作之间顺序执行多个操作。在每个操作中，可以使用 `.then()` 方法返回一个新的 Promise，从而在异步流程中继续执行下一个操作。这样可以避免回调函数地狱，提高代码的可读性和可维护性。

链式调用的基本步骤包括：

创建一个新的 Promise 对象，并调用 `resolve` 或 `reject` 来变更其状态。
在 `then` 或 `catch` 方法中，处理成功或失败的状态。
在 `then` 方法中，可以使用 `return` 关键字返回一个新的 Promise 对象，或者直接返回普通值。
继续在下一个 `then` 方法中处理返回的 Promise 对象，或者直接处理返回的普通值。
例如，以下代码展示了如何使用 `.then()` 和 `.catch()` 方法进行链式调用：

```
let promise1 = new Promise((resolve, reject) => {
    resolve('new promise111111');
});
 
promise1.then(res => {
    console.log(res); // 输出: 'new promise111111'
    return '链式调用的方式';
  }).then(value => {
        console.log(value); // 输出: '链式调用的方式'
});
```

在这个例子中，`promise1` 被成功地 resolve 并返回了一个值，然后 `then` 方法被调用，返回了一个新的 Promise 对象，并返回了 `'链式调用的方式'`。这个新的 Promise 对象又被继续 `.then` 处理，最终返回了 `'链式调用的方式'`。

需要注意的是，每次 `.then` 方法调用都会返回一个新的 Promise 对象，因此链式调用的结果取决于最后 `.then` 方法中返回的值或新的 Promise 对象。

错误捕获
Promise.prototype.catch用于指定Promise状态变为rejected时的回调函数，可以认为是.then的简写形势，返回值跟.then一样

```
let obj = new Promise((resolve,reject) => {
    reject('error');
});
 
obj.catch(result => {
    console.log(result);
})
```

async、await的用法
特点简洁：异步编程的最高境界就是不关心它是否是异步。async、await很好的解决了这一点，将异步强行转换为同步处理。
async/await与promise不存在谁代替谁的说法，因为async/await是寄生于Promise，Generater的语法糖。

用法
async用于申明一个function是异步的，而await可以认为是async wait的简写，等待一个异步方法执行完成。
规则：
1 async和await是配对使用的，await存在于async的内部。否则会报错
2 await表示在这里等待一个promise返回，再接下来执行
3 await后面跟着的应该是一个promise对象，（也可以不是，如果不是接下来也没什么意义了…）

写法

```js
async function demo() {
    let a= await sleep(100); //上一个await执行之后才会执行下一句
    let b= await sleep(a+ 100);
    let c= await sleep(b+ 100);
    return c; // console.log(c);
}
 
demo().then(result => {
    console.log(result);
});
```

- **错误捕获**

如果是reject状态，可以用try-catch捕捉

```js
let obj = new Promise((resolve,reject) => {
    setTimeout(() => {
        reject('error');
    },1000);
});
 
async function demo(item) {
    try {
        let result = await obj;
    } catch(e) {
        console.log(e);
    }
}
 
demo();
```

两者区别
1、promise是ES6，async/await是ES7
2、async/await相对于promise来讲，写法更加优雅
3、reject状态：
    （1）promise错误可以通过catch来捕捉，建议尾部捕获错误，
    （2）async/await既可以用.then又可以用try-catch捕捉

推荐一篇Promise文章：https://juejin.cn/post/6844904181627781128

### 23、JS中数组常用方法详解 

顺序	方法名	功能	返回值	是否改变原数组	版本
1	push()	(在结尾)向数组添加一或多个元素	返回新数组长度	Y	ES5-
2	unshift()	（在开头)向数组添加一或多个元素	返回新数组长度	Y	ES5-
3	pop()	删除数组的最后一位	返回被删除的数据	Y	ES5-
4	shift()	移除数组的第一项	返回被删除的数据	Y	ES5-
5	reverse()	反转数组中的元素	返回反转后数组	Y	ES5-
6	sort()	以字母顺序(字符串Unicode码点)对数组进行排序	返回新数组	Y	ES5-
7	splice()	在指定位置删除指定个数元素再增加任意个数元素 （实现数组任意位置的增删改)	返回删除的数据所组成的数组	Y	ES5-
8	concat()	通过合并（连接）现有数组来创建一个新数组	返回合并之后的数组	N	ES5-
9	join()	用特定的字符,将数组拼接形成字符串 (默认",")	返回拼接后的新数组	N	ES5-
10	slice()	裁切指定位置的数组	被裁切的元素形成的新数组	N	ES5-
11	toString()	将数组转换为字符串	新数组	N	ES5-
12	valueOf()	查询数组原始值	数组的原始值	N	ES5-
13	indexOf()	查询某个元素在数组中第一次出现的位置	存在该元素,返回下标,不存在 返回 -1	N	ES5-
14	lastIdexOf()	反向查询数组某个元素在数组中第一次出现的位置	存在该元素,返回下标,不存在 返回 -1	N	ES5-
15	forEach()	(迭代) 遍历数组,每次循环中执行传入的回调函数	无/(undefined)	N	ES5-
16	map()	(迭代) 遍历数组, 每次循环时执行传入的回调函数,根据回调函数的返回值,生成一个新的数组	有/自定义	N	ES5-
17	filter()	(迭代) 遍历数组, 每次循环时执行传入的回调函数,回调函数返回一个条件,把满足条件的元素筛选出来放到新数组中	满足条件的元素组成的新数组	N	ES5-
18	every()	(迭代) 判断数组中所有的元素是否满足某个条件	全都满足返回true 只要有一个不满足 返回false	N	ES5-
19	some()	(迭代) 判断数组中是否存在,满足某个条件的元素	只要有一个元素满足条件就返回true,都不满足返回false	N	ES5-
20	reduce()	(归并)遍历数组, 每次循环时执行传入的回调函数,回调函数会返回一个值,将该值作为初始值prev,传入到下一次函数中	最终操作的结果	N	ES5-
21	reduceRight()	(归并)用法同reduce,只不过是从右向左	同reduce	N	ES5-
22	includes()	判断一个数组是否包含一个指定的值.	是返回 true，否则false	N	ES6
23	Array.from()	接收伪数组,返回对应的真数组	对应的真数组	N	ES6
24	find()	遍历数组,执行回调函数,回调函数执行一个条件,返回满足条件的第一个元素,不存在返回undefined	满足条件第一个元素/否则返回undefined	N	ES6
25	findIndex()	遍历数组,执行回调函数,回调函数接受一个条件,返回满足条件的第一个元素下标,不存在返回-1	满足条件第一个元素下标,不存在=>-1	N	ES6
26	fill()	用给定值填充一个数组	新数组	Y	ES6
27	flat()	用于将嵌套的数组“拉平”，变成一维的数组。	返回一个新数组	N	ES6
28	flatMap()	flat()和map()的组合版 , 先通过map()返回一个新数组,再将数组拉平( 只能拉平一次 )	返回新数组	N	ES6



**二、不改变原数组的方法**

1.concat（）  合并数组

2.join（） 数组转字符串 语法:数组名.join('连接符') 作用: 就是把一个数组转成字符串

find()返回匹配的元素，findIndex()返回匹配元素的索引，find()如果没有匹配到元素则返回undefined, 而findIndex()返回-1

4、instanceof( )

`instanceof`[运算符](https://so.csdn.net/so/search?q=运算符&spm=1001.2101.3001.7020)返回一个布尔值，如下：