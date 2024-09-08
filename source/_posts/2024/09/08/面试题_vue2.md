---
title: 前端面试题 - VUE2篇
categories: [前端,面试题,VUE2]
tags: [前端,面试题,VUE2 ]
date: 2024-09-08
---

前端面试题 - VUE2篇

<!--more-->

## 四、Vue2篇

### Vue中的的通信方式有几种？隔代组件的通信你用那种方式解决？

`props/$emit `适用父子组件通信
`ref与parent/children`适用父子组件通信
`attrs/listeners,provide/inject `适用于隔代组件通信
`vuex,EventBus(事件总线) `适用于父子、隔代、兄弟组件通信
slot插槽方式

### v-show 和 v-if指令的共同点和不同点？

v-show是css切换，v-if是完整的销毁和重新创建，如果频繁切换时用v-show，运行时较少改变用v-if

### 为什么使用key？

做一个唯一标识， Diff 算法就可以正确的识别此节点。作用主要是为了高效的更新虚拟 DOM。

### 简述computed和watch有什么区别

**computed**

- 当依赖的响应式数据变化时，会重新计算并返回新的值。
- 支持缓存。只有当其依赖的响应式属性发生变化时，才会重新计算。
- 这意味着只要依赖不变，多次访问computed属性会立即返回之前的计算结果，而无需再次执行计算函数。
- 不支持异步操作。computed属性内的函数必须是同步的。
- 必须返回一个值，这个值就是computed属性的值。
- 页面刚刚挂载（即Vue实例创建并挂载到DOM上）时就开始监听其依赖的响应式数据。
- 可以有多个依赖，即一个计算属性可以依赖于多个响应式数据。

**watch**（侦听器）

- 用于观察和响应Vue实例上数据的变化。
- 当需要在数据变化时执行异步或开销较大的操作时，watch非常有用
- 不支持缓存。每次监听的数据发生变化时，都会执行对应的回调函数。
- 支持异步操作。在watch的回调函数中，可以执行异步任务，如数据请求等。

- 不需要返回值。它主要通过回调函数来响应数据的变化。
- 默认情况下，不会在页面挂载时立即监听数据变化。但可以通过设置`immediate: true`来让watch在初始化时立即执行一次回调函数。
- 主要针对单个响应式数据的变化进行监听。当然，也可以在watch的回调函数中处理多个数据的变化，但这与computed的依赖机制不同。

### 简述computed和watch的使用场景

computed:
支持缓存，数据变，直接会触发相应的操作；
监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值；
当一个属性发生变化时，需要执行对应的操作；即一个属性受多个属性影响，多对一或者一对一的关系；
监听的是这个属性自身的变化，且不会操作缓存
监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数，
是一个计算属性,类似于过滤器,对绑定到view的数据进行处理
　　　　当一个属性受多个属性影响的时候就需要用到computed
　　　　最典型的例子： 购物车商品结算的时候
watch:
1.是观察的动作，
2.应用：监听props，$emit或本组件的值执行异步操作
3.无缓存性，页面重新渲染时值不变化也会执行
watch是一个观察的动作
　　　　当一条数据影响多条数据的时候就需要用watch
　　　　例子：搜索数据

### Vue实例的生命周期讲一下, mounted阶段真实DOM存在了嘛?

Vue实例从创建到销毁的过程，就是生命周期。
也就是：开始创建->初始化数据->编译模板->挂载dom->数据更新重新渲染虚拟 dom->最后销毁。这一系列的过程就是vue的生命周期。所以在mounted阶段真实的DOM就已经存在了。
beforeCreate：vue实例的挂载元素el和数据对象data都还没有进行初始化，还是一个 undefined状态
created: 此时vue实例的数据对象data已经有了，可以访问里面的数据和方法， el还没有，也没有挂载dom
beforeMount: 在这里vue实例的元素el和数据对象都有了，只不过在挂载之前还是虚拟的dom节点
mounted: vue实例已经挂在到真实的dom上，可以通过对 dom操作来获取dom节点
beforeUpdate: 响应式数据更新时调用，发生在虚拟dom打补丁之前，适合在更新之前访问现有的 dom，比如手动移除已添加的事件监听器
updated: 虚拟dom重新渲染和打补丁之后调用，组成新的 dom已经更新，避免在这个钩子函数中操作数据，防止死循环。
activated: 当组件keep-alive激活时被调用
deactivated:当组件keep-alive停用时被调用
beforeDestroy: vue实例在销毁前调用，在这里还可以使用，通过this也能访问到实例，可以在这里对一些不用的定时器进行清除，解绑事件。
destroyed：vue实例销毁后调用，调用后所有事件监听器会被移除，所有的子实例都会被销毁。

### 1、什么是前端构建工具?比如（Vue2的webpack，Vue3的Vite）

①首先要明白：浏览器它只认识html, css, js
企业级项目里都可能会具备哪些功能？

1. typescript:·如果遇到ts文件我们需要使用tsc将typescript代码转换为js代码
2. React/Vue:安装react-compiler / vue-complier，将我们写的jsx文件或者.vue文件转换为render函数
3. less/postcss/component-style:我们又需要安装less-loader, sass-loader等一系列编译工具4. 语法降级:babel --->将es的新语法转换旧版浏览器可以接受的语法
4. 体积优化:uglifyjs --->将我们的代码进行压缩变成体积更小性能更高的文件
   ②前因后果：
   因为稍微改一点点东西，非常麻烦！
   将App.tsx --->tsc --->· App.jsx --->React-complier --->js文件
   但是有一个东西能够帮你把tsc,react-compiler, less, babel,uglifyjs全部集成到一起，我们只需要关心我们写的代码就好了
   我们写的代码变化-->有人帮我们自动去tsc, react-compiler,less, babel, uglifyjs全部挨个走一遍-->js
   这个东西就叫做 前端构建工具
   ③前端构建工具的工作：
   打包: 将我们写的浏览器不认识的代码交给构建工具进行编译处理的过程就叫做打包，打包完成以后会给我们一个浏览器可以认识的文件。
   一个构建工具他到底承担了哪些脏活累活:
   1.模块化开发支持：支持直接从node_modules里引入代码＋多种模块化支持
   2.处理代码兼容性：比始babel语法降级，less,ts 语法转换(**不是构建工具做的，构建工具将这些语法对应的处理工具集成进来自动化处理)
   3.提高项目性能：压缩文件，**代码分割*
   4.优化开发体验:
   （1）构建工具会帮你自动监听文件的变化，当文件变化以后自动帮你调用对应的集成工具进行重新打包，然后再浏览器重新运行（整个过程叫做热更新, hot replacement）
   （2）开发服务器:跨域的问题，用react-cli create-react-element-vue-cli·解决跨域的问题,
   ④构建工具总结： 
        构建工具它让我们可以不用每次都关心我们的代码在浏览器如何运行，我们只需要首次给构建工具提供一个配置文件(这个配置文件也不是必须的，如果你不给他他会有默认的帮你去处理)，有了这个集成的配置文件以后，我们就可以在下次需要更新的时候调用一次对应的命令就好了，如果我们再结合热更新，我们就更加不需要管任何东西，这就是构建工具去做的东西。
   构建工具它让我们不用关心生产的代码，也不用去关心代码如何在浏览器运行，只需要关心我们的开发怎么写的爽怎么写就好了

### 2、Vue 组件之间的通信方式

**（1）父组件向子组件传值**

绑定

**（2）子组件向父组件传值**

$emit

### 3、Vuex的理解及使用场景

Vuex 是一个专为 Vue 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。

Vuex 的状态存储是响应式的；当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新 2. 改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation， 这样使得我们可以方便地跟踪每一个状态的变化 Vuex主要包括以下几个核心模块：

1.State：定义了应用的状态数据

2.Getter：在 store 中定义“getter”（可以认为是 store 的计算属性），

就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来， 且只有当它的依赖值发生了改变才会被重新计算

3. Mutation：是唯一更改 store 中状态的方法，且必须是同步函数

4. Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作

5. Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中

### 4、vue 的生命周期 八个阶段

1.beforeCreate

在实例创建之间执行，数据是未加载状态。

创建一个Vue实例，此时实例上只有一些生命周期函数和默认的事件

此时data computed watch methods上的方法和数据均不能访问

2.created

在实例创建、数据加载后，能初始化数据，DOM渲染之前执行。

可以对data数据进行操作，可进行一些请求，请求不易过多，避免白屏时间太长。

3.beforeMount

虚拟DOM已创建完成，在数据渲染前最后一次更改数据。el未挂载。

判断el的挂载方式

判断是否有template设置

将template进行渲染保存到内存当中，还未挂载在页面上

4.mounted

页面、数据渲染完成。el挂载完毕。可以访问DOM节点。

将内存中的模版挂载到页面上

此时可以操作页面上的DOM节点

此时组件从创建阶段进入运行阶段

5.beforeUpdate

重新渲染之前触发。不会造成重渲染。

页面显示的数据是旧的，此时data里面的数据是最新，页面数据和data数据暂未同步6.updated

数据已经更新完成，DOM也重新render完成，更改数据会陷入死循环。

根据data里的最新数据渲染出最新的DOM树，然后将最新的DOM挂载到页面

此时data和页面数据一致，都是最新的

7.beforeDestroy

实例销毁前执行，实例仍然完全可用。

此时组件从运行阶段进入到销毁阶段

组件上的data和methods以及过滤器等都出于可用状态，销毁还未执行

8.Destroyed 

实例销毁后执行，这时候只剩下DOM空壳。

组件已经被完全销毁，组件中所有的数据、方法、指令、过滤器等，都已不可用




### 5、简述Vue每个周期具体适合哪些场景？

beforeCreate： 在new一个vue实例后，只有一些默认的生命周期钩子和默认事件，其他的东西都还没创建。在beforeCreate生命周期执行的时候，data和methods中的数据都还没有初始化。不能在这个阶段使用data中的数据和methods中的方法
created： data 和 methods都已经被初始化好了，如果要调用 methods 中的方法，或者操作 data 中的数据，最早可以在这个阶段中操作
beforeMount： 执行到这个钩子的时候，在内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的
mounted： 执行到这个钩子的时候，就表示Vue实例已经初始化完成了。此时组件脱离了创建阶段，进入到了运行阶段。如果我们想要通过插件操作页面上的DOM节点，最早可以在和这个阶段中进行
beforeUpdate： 当执行这个钩子时，页面中的显示的数据还是旧的，data中的数据是更新后的， 页面还没有和最新的数据保持同步
updated： 页面显示的数据和data中的数据已经保持同步了，都是最新的
beforeDestory： Vue实例从运行阶段进入到了销毁阶段，这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于可用状态。还没有真正被销毁
destroyed： 这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于不可用状态。组件已经被销毁了。



### 6、简述MVVM 和MVC的原理以及区别？

MVVM视图模型双向绑定，是Model-View-ViewModel的缩写

1、MVVM的优点：

低耦合。视图（View）可以独立于Model变化和修改，一个Model可以绑定到不同的View上，当View变化的时候Model可以不变化，当Model变化的时候View也可以不变；
可重用性。你可以把一些视图逻辑放在一个Model里面，让很多View重用这段视图逻辑。
独立开发。开发人员可以专注于业务逻辑和数据的开发(ViewModel)，设计人员可以专注于页面设计。
可测试。



2、什么是MVC?

        MVC是应用最广泛的软件架构之一,一般MVC分为:Model(模型),View(视图),Controller(控制器)。 这主要是基于分层的目的,让彼此的职责分开.View一般用过Controller来和Model进行联系。Controller是Model和View的协调者,View和Model不直接联系。基本都是单向联系。M和V指的意思和MVVM中的M和V意思一样。C即Controller指的是页面业务逻辑。MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下。

Model（模型）表示应用程序核心（如数据库）。

View（视图）显示效果（HTML页面）。

Controller（控制器）处理输入（业务逻辑）。

MVC 模式同时提供了对 HTML、CSS 和 JavaScript 的完全控制。

Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。（通常模型对象负责在数据库中存取数据)

View（视图）是应用程序中处理数据显示的部分。(通常视图是依据模型数据创建的)

Controller（控制器）是应用程序中处理用户交互的部分。(通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据）

优点:

1、低耦合

2、重用性高

3、生命周期成本低

4、部署快

5、可维护性高

6、有利软件工程化管理
3、MVC与MVVM的区别？

        MVC和MVVM的区别并不是VM完全取代了C，ViewModel存在目的在于抽离Controller中展示的业务逻辑，而不是替代Controller，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重用。

MVC中Controller演变成MVVM中的ViewModel
MVVM通过数据来显示视图层而不是节点操作
MVVM主要解决了MVC中大量的dom操作使页面渲染性能降低,加载速度变慢,影响用户体验



### 7、vue常见指令

v-model 多用于表单元素实现双向数据绑定
v-bind：简写为冒号：“：”，动态绑定一些元素的属性，类型可以是：字符串、对象或数组。
v-on:click 给标签绑定函数，可以缩写为：“@”，例如绑定一个点击函数 函数必须写在methods里面
v-for 格式： v-for="字段名 in(of) 数组json" 循环数组或json，记得加上key
v-show 显示内容
v-if 指令：取值为true/false，控制元素是否需要被渲染
v-else 指令：和v-if指令搭配使用，没有对应的值。当v-if的值false，v-else才会被渲染出来
v-else-if 必须和v-if连用
v-text 解析文本
v-html 解析html标签 （一般常见的解决后台的富文本内容）
v-bind:class 三种绑定方法

1. 对象型 "{red：isred}"
2. 三元型 " isred？"red"："blue"
3. 数组型 " [{red："isred" }，{blue："isblue"}] "

v-once 进入页面时 只渲染一次 不在进行渲染
v-cloak 防止闪烁
v-pre 把标签内部的元素原位输出





### 8、vue中的data为什么是一个函数？起到什么作用？ 

**在Vue组件中，data选项必须是一个函数，而不能直接是一个对象。这是因为Vue组件可以同时存在多个实例，如果直接使用对象形式的data选项，那么所有的实例将会共享同一个data对象，这样就会造成数据互相干扰的问题。**
因此，将data选项设置为函数可以让每个实例都拥有自己独立的data对象。当组件被创建多次时，每个实例都会调用该函数并返回一个新的data对象，从而保证了数据的隔离性。
另外，data选项作为一个函数还具有一个重要的特性，就是它可以接收一个参数，这个参数是组件实例本身。这个特性在一些场景下非常有用，例如在定义组件时需要使用组件实例的一些属性或方法来计算初始数据。
因此，为了避免数据共享和保证数据隔离性，以及方便使用组件实例的属性和方法，Vue组件中的data选项必须是一个函数。
以下是一个简单的Vue组件示例，其中data被定义为一个函数：

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="increment">{{ count }}</button>
  </div>
</template>
 
<script>
export default {
  data() {
    return {
      message: 'Hello, Vue!',
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>
```

在这个例子中，data函数返回一个包含message和count两个属性的对象。每次创建组件实例时，Vue都会调用该函数返回一个新的数据对象，确保每个组件实例都有它自己的数据对象。

### 9、vue中ref的作用？  

1、**获取DOM元素的引用**。

ref 加在普通的元素上，用this.ref.name 获取到的是dom元素

vue给我们提供一个操作dom的属性，ref绑定在dom元素上时，用起来与id差不多，通过this.$refs来调用



2、获取子组件的引用。

在Vue组件中使用ref可以获取子组件的引用，从而可以在父组件内部调用子组件的方法或访问其数据。
在父组件中将子组件引入，并在子组件标签上添加ref属性，然后就可以通过this.$refs.myChild获取子组件的引用，在父组件内部调用子组件的sayHello方法。



3、利用 v-for 和 ref 获取一组数组或者dom 节点



注意事项：

ref 需要在dom渲染完成后应用，在使用时确保dom已经渲染完成。比如在生命周期 mounted(){} 钩子中调用，或者在 this.$nextTick(()=>{}) 中调用。

如果ref 是循环出来的，有多个重名，那么ref值会是一个数组 ，此时要拿到单个ref 只需要循环就可以。




### 10、vue中hash和history的区别 ？  

Vue-router的路由分为hash和history模式

1、hash方式

hash方式是指url中存在 # 的一种方式，是vueRouter的默认模式，
当#后面的url地址发生变化时，浏览器不会向服务器发送请求，故不会刷新页面
当#后面的url地址发生变化时，会触发hashChange（hash模式得核心实现原理）事件，从而，我们可以通过监听hashChange事件来知道路由发生变化，从而进一步去更新我们的页面
只可修改hash部分，
当浏览器刷新时，浏览器只会向服务器去请求# 前面的域名服务器下根目录下的index.html文件
hash模式会创建hashHistory对象,hashHistory对象有两个方法，push() 和 replace()
HashHistory.push()会将新的路由添加到浏览器访问的历史的栈顶,而HasHistory.replace()会替换到当前栈顶的路由
2、history模式

history模式得路由和域名之间直接通过/连接，无#符分隔，就是普通url形式
history模式当发生路由跳转时，通过HTML5的history.pushState()方法或者history.replaceState() 方法改变地址栏地址，并将地址的改变记录到浏览器访问栈中。（这里有一点需要注意，它只改变了浏览器地址栏中的地址，但并不会像服务器去发送请求）
当浏览器前进，后台，或者调用back(),forward(), go()等方法时，会触发popstate事件。故，我们可以通过监听popstate事件来获取最新的路由地址，从而更新页面
通过pushstate() 修改的url可以是与当前url同源的任意url。
需要和服务器配合使用，否则容易出现页面404的情况
总结如下：

hash模式带#号比较丑，history模式比较优雅；
pushState设置的新的URL可以是与当前URL同源的任意URL；而hash只可修改#后面的部分，故只可设置与当前同文档的URL；
pushState设置的新URL可以与当前URL一模一样，这样也会把记录添加到栈中；而hash设置的新值必须与原来不一样才会触发记录添加到栈中；
pushState通过stateObject可以添加任意类型的数据到记录中；而hash只可添加短字符串；
pushState可额外设置title属性供后续使用；
hash兼容IE8以上，history兼容IE10以上；
history模式需要后端配合将所有访问都指向index.html，否则用户刷新页面，会导致404错误。
