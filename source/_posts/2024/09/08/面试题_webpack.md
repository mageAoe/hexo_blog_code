---
title: 前端面试题 - Webpack篇
categories: [前端,面试题,Webpack]
tags: [前端,面试题,Webpack ]
date: 2024-09-08
---

前端面试题 - Webpack篇

<!--more-->

## Webpack篇

### 1、Webpack是什么？

> webpack 是一个静态模块打包器，当 webpack 处理应用程序时，会递归构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将这些模块打包成一个或多个 bundle。
> webpack 就像一条生产线,要经过一系列处理流程(loader)后才能将源文件转换成输出结果。 这条生产线上的每个处理流程的职责都是单一的,多个流程之间有存在依赖关系,只有完成当前处理后才能交给下一个流程去处理。
> 插件就像是一个插入到生产线中的一个功能,在特定的时机对生产线上的资源做处理。 webpack 在运行过程中会广播事件,插件只需要监听它所关心的事件,就能加入到这条生产线中,去改变生产线的运作。

### 2、Webpack的打包过程/打包原理/构建流程？ 

1. 初始化

   - **读取配置**：Webpack 从配置文件（如 `webpack.config.js`）中读取配置，获取入口、输出、加载器（loader）、插件（plugin）等信息。

   - **初始化插件**：根据配置初始化插件，插件可以在 Webpack 生命周期的不同阶段执行特定任务。

   - **创建编译对象**：Webpack 创建一个 `Compiler` 对象，它包含了整个构建过程的核心逻辑和钩子（hooks）。

2. 入口解析

   - **确定入口点**：Webpack 从配置的入口点（entry）开始解析，创建一个 `Compilation` 对象，这是一次构建的上下文。

   - **生成模块依赖图**：从入口文件开始，递归解析所有依赖的模块，构建模块依赖图。

3. 模块解析

   - **递归解析模块**：Webpack 使用 `loader` 处理不同类型的模块（如 JavaScript、CSS、图片等），将其转换为可以被 Webpack 处理的格式。

   - **模块化处理**：对于每个模块，Webpack 会生成一个对应的模块对象，记录模块的依赖、生成的代码等信息。

4. 模块转换

   - **应用 Loader**：根据配置的 `loader` 对模块进行转换。例如，使用 `babel-loader` 将 ES6 转换为 ES5，使用 `css-loader` 和 `style-loader` 处理 CSS 文件。

   - **生成模块对象**：转换后的每个模块会生成一个模块对象，包含模块 ID、依赖关系、转换后的代码等信息。

5. 生成依赖关系图

   - **构建依赖图**：Webpack 通过解析模块的依赖，递归地构建出一个模块依赖关系图。

   - **处理依赖关系**：Webpack 处理这些依赖关系，确定模块的加载顺序。

6. 优化（可选）

   - **代码拆分**：使用代码拆分（Code Splitting）将代码分割成多个 bundle 文件，以实现按需加载。

   - **树摇（Tree Shaking）**：移除未使用的代码，减小打包体积。

   - **压缩代码**：使用 `TerserPlugin` 等插件压缩 JavaScript 代码，减小文件大小。

7. 输出

   - **生成 Bundle**：Webpack 根据依赖关系图将所有模块打包到一个或多个 bundle 文件中。

   - **写入文件系统**：将生成的 bundle 文件写入到配置的输出目录中。

8. 插件处理
   - **执行插件钩子**：在整个构建过程中，Webpack 会在不同阶段执行插件钩子（hooks），以完成各种任务，例如代码压缩、资源注入、生成 HTML 文件等。

### 3、Webpack中loader的作用/ loader是什么？

Loader 是webpack中提供了一种处理多种文件格式的机制，因为webpack只认识JS和JSON，所以Loader相当于翻译官，将其他类型资源进行预处理。
用于对模块的"源代码"进行转换。
loader支持链式调用，原型调用的顺序是从右往左。原型链中的每个loader会处理之前已处理过的资源，最终变为js代码。
可以通过 loader 的预处理函数，为 JavaScript 生态系统提供更多能力。

### 4、常见的loader有哪些？

less-loader: 将less文件编译成css文件，开发中，我们常常会使用less预处理器编写css样式，使开发效率提高
css-loader: 将css文件变成commonjs模块加载到js中，模块内容是样式字符串
style-loader:  创建style标签，将js中的样式资源插入标签内，并将标签添加到head中生效
ts-loader:  打包编译Typescript文件

### 5、Plugin有什么作用？Plugin是什么？

- Plugin功能更强大，主要目的就是解决loader 无法实现的事情，比如打包优化和代码压缩等。
- Plugin加载后，在webpack构建的某个时间节点就会触发plugin定义的功能，帮助webpack做一些事情。实现对webpack的功能扩展。

### 6、常见的Plugin有哪些？

html-webpack-plugin 处理html资源，默认会创建一个空的HTML，自动引入打包输出的所有资源（js/css）
mini-css-extract-plugin 打包过后的css在js文件里，该插件可以把css单独抽出来
clean-webpack-plugin 每次打包时候，CleanWebpackPlugin 插件就会自动把上一次打的包删除

### 7、Webpack中Loader和Plugin的区别

webpack 就像一条生产线,要经过一系列处理流程(loader)后才能将源文件转换成输出结果。 这条生产线上的每个处理流程的职责都是单一的,多个流程之间有存在依赖关系,只有完成当前处理后才能交给下一个流程去处理。



插件就像是一个插入到生产线中的一个功能,在特定的时机对生产线上的资源做处理。 webpack 在运行过程中会广播事件,插件只需要监听它所关心的事件,就能加入到这条生产线中,去改变生产线的运作。

### 8、如何利用webpack来优化前端性能？

- 代码压缩

- **按需加载**

  - 代码分割 splitChunks - 在optimization配置项中配置
  - 使用Dll进行分包
  - 使用路由懒加载


### 9、Webpack如何配置压缩代码？压缩了什么？

在optimization配置项中来配置该插件作为压缩器进行压缩。
在plugins里使用该插件进行压缩
js压缩:利用terser-webpack-plugin
css压缩:利用了optimize-css-assets-webpack-plugin 插件
删除了console、注释、空格、换行、没有使用的css代码等

### 10、如何提高webpack的构建速度？ 

思路1：减少需要构建的文件或代码

HMR(Hot Module Replacement) 模块热替换只重新构建发生变化的模块 – 开发环境中
缩小处理范围:合理利用这两个属性exclude：不需要处理的文件 和 include：需要处理的文件
babel缓存 第二次构建时，会读取之前的缓存，只重新构建变化的文件
使用Dll进行分包 --> 分包方便按需加载

> 正常情况下node_module会被打包成一个文件
> 使用dll技术，对可以将那些不常更新的框架和库进行单独打包，生成一个chunk
> 项目源代码也需要拆分，可以根据路由来划分打包文件 --> 怎么实现路由懒加载？webpack中如何实现组件异步加载？

思路2：多进行进行构建 

- 多进程打包 thread-loader,将其放在费时的loader之前

> 进程启动和进程通信都有开销，工作时间比较长，才需要多进程打包