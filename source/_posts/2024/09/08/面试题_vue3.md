---
title: 前端面试题 - VUE3篇
categories: [前端,面试题,VUE3]
tags: [前端,面试题,VUE3 ]
date: 2024-09-08
---

前端面试题 - VUE3篇

<!--more-->

## 五 、Vue3篇

### 谈谈你对Vue3 Composition API有什么理解

Vue 3 的 Composition API 是一个用于组织组件逻辑的新 API，旨在解决 Vue 2 中使用 Options API 时遇到的一些问题，如逻辑复用和大型组件的复杂性。Composition API 提供了一种更灵活和可扩展的方式来定义组件逻辑，使得代码更加清晰和易于维护。



Vue 3 的 Composition API 提供了一种新的方式来组织和复用组件逻辑，增强了代码的可读性和可维护性。它通过 `setup` 函数、响应式 API、计算属性和侦听器以及生命周期钩子等机制，使得组件逻辑更加清晰和模块化，特别适用于大型项目和复杂组件。



### 1、Vue2.0和Vue3.0的区别？

1. **性能提升**

- **编译器优化**：Vue 3.0 的模板编译器进行了优化，生成的代码更高效。
- **代理机制**：Vue 3.0 使用 `Proxy` 代替了 Vue 2.0 中的 `Object.defineProperty` 实现响应式系统，从而解决了 Vue 2.0 中的一些限制，如无法检测数组索引和对象新增属性的变化。
- **更快的渲染**：通过更高效的 diff 算法和优化的组件更新机制，Vue 3.0 提供了更快的渲染性能。

2. **Composition API**

- **灵活性**：Vue 3.0 引入了 Composition API，它是一组新的 API，用于在函数组件中组合逻辑，使代码更具可维护性和复用性。
- **可读性**：Composition API 允许将相关的逻辑组合在一起，而不是分散在生命周期钩子中，从而提高了代码的可读性和组织性。

3. **新的生命周期钩子**

- **onBeforeMount、onMounted、onBeforeUpdate、onUpdated、onBeforeUnmount、onUnmounted**：Vue 3.0 为 Composition API 引入了一组新的生命周期钩子，替代了 Vue 2.0 中的生命周期钩子。

4. **更好的 TypeScript 支持**

- **内置类型声明**：Vue 3.0 提供了内置的 TypeScript 类型声明，改善了与 TypeScript 的集成，使得开发者可以更好地利用 TypeScript 的静态类型检查和自动补全功能。

5. **Tree-shaking 支持**

- **模块化设计**：Vue 3.0 采用了基于 ES 模块的设计，允许进行更有效的 tree-shaking，从而减小打包后的文件大小。

6. **Fragment 和 Teleport**

- **Fragment**：Vue 3.0 支持组件返回多个根元素，从而不需要为多个元素包裹一个单一的根元素。
- **Teleport**：允许将组件的模板渲染到 DOM 树中的任意位置，而不仅仅是父组件的 DOM 层次结构中。

7. **Suspense**

- **异步组件**：Vue 3.0 引入了 `Suspense` 组件，用于处理异步组件的加载状态，可以在异步操作完成前显示一个占位符。



### 2、Vue3带来了什么改变？ 

性能的提升

打包大小减少41%

初次渲染快55%, 更新渲染快133%

内存减少54%

源码的升级

使用Proxy代替defineProperty实现响应式

重写虚拟DOM的实现和Tree-Shaking

拥抱TypeScript

Vue3可以更好的支持TypeScript

新的特性

Composition API（组合API）
（1）setup配置

（2）ref与reactive

（3）watch与watchEffect

（4）provide与inject

新的内置组件
（1）Fragment

（2）Teleport

（3）Suspense

其他改变
（1）新的生命周期钩子

（2）data 选项应始终被声明为一个函数

（3）移除keyCode支持作为 v-on 的修饰符


### 3、生命周期（vue2和vue3的生命周期对比）有哪些？

Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：

beforeDestroy改名为 beforeUnmount
destroyed改名为 unmounted
Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：

beforeCreate===>setup()
created=======>setup()
beforeMount ===>onBeforeMount
mounted=======>onMounted
beforeUpdate===>onBeforeUpdate
updated =======>onUpdated
beforeUnmount ==>onBeforeUnmount
unmounted =====>onUnmounted

### 4、Vue3.0中的响应式原理是什么？vue2的响应式原理是什么？

vue2.x的响应式

**实现原理：**
	对象类型：通过Object.defineProperty()对属性的读取、修改进行拦截（数据劫持）。
	数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。
                                   Object.defineProperty(data, 'count', {
                                                                  get () {}, 
                                                                   set() {}
                                    })

**存在问题：**
	新增属性、删除属性, 界面不会更新。
	直接通过下标修改数组, 界面不会自动更新。

Vue3.0的响应式

**实现原理:**
	通过Proxy（代理）: 拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
	通过Reflect（反射）: 对源对象的属性进行操作。
	MDN文档中描述的Proxy与Reflect：

```js
Proxy：Proxy - JavaScript | MDN
Reflect：Reflect - JavaScript | MDN
new Proxy(data, {
    // 拦截读取属性值
    get (target, prop) {
        return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
        return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
        return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```

vue3响应式数据的判断
	isRef: 检查一个值是否为一个 ref 对象
	isReactive: 检查一个对象是否是由 reactive 创建的响应式代理
	isReadonly: 检查一个对象是否是由 readonly 创建的只读代理

### 5、vue3的常用 Composition API有哪些？

1.拉开序幕的setup

理解：Vue3.0中一个新的配置项，值为一个函数。
setup是所有Composition API（组合API）“ 表演的舞台 ”。
组件中所用到的：数据、方法等等，均要配置在setup中。
setup函数的两种返回值：
   若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重关注！）
   若返回一个渲染函数：则可以自定义渲染内容。（了解）
     5.setup的几个注意点

 setup执行的时机
 在beforeCreate之前执行一次，this是undefined。
setup的参数
props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
context：上下文对象
attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 this.$attrs。
slots: 收到的插槽内容, 相当于 this.$slots。
emit: 分发自定义事件的函数, 相当于 this.$emit。
尽量不要与Vue2.x配置混用





### 6、Vue3中的ref函数

作用: 定义一个响应式的数据

语法: const xxx = ref(initValue)

​	创建一个包含响应式数据的引用对象（reference对象，简称ref对象）。

​	JS中操作数据： xxx.value

​	模板中读取数据: 不需要.value，直接：<div>{{xxx}}</div>

备注：
	接收的数据可以是：基本类型、也可以是对象类型。

​	基本类型的数据：响应式依然是靠Object.defineProperty()的get与set完成的。

​	对象类型的数据：内部 “ 求助 ” 了Vue3.0中的一个新函数—— reactive函数。