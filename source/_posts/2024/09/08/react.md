---
title: react的一些使用记录
categories: react
tags: react
date: 2024-09-08
---

react平时使用的记录

<!--more-->

## setState 传入回调函数场景

假设 age 为 42，这个方法将会调用 setAge(age + 1) 三次。

```js
function handleClick() {
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
}
```

然而，触发 handleClick 方法后，age 还是 43，不是 45。因为 set 方法不会更新 age 状态变量在当前正在运行的代码。所以每次 setAge(age + 1) 都变成 setAge(43)。

为了解决这个问题，你可以传一个函数类型参数给 setAge，获取到下一会的状态。

```js
function handleClick() {
  setAge(a => a + 1); // setAge(42 => 43)
  setAge(a => a + 1); // setAge(43 => 44)
  setAge(a => a + 1); // setAge(44 => 45)
}
```

`a => a + 1` 是你的更新函数。它接收一个待改变的 state，然后在函数体计算返回下一个 state。

React 把更新函数放入一个队列中。然后在下一次 render 时，更新函数将会以相同的顺序被调用。

1. a => a + 1 将接收 42 作为待改变的状态，然后返回 43 作为下一次的状态。
2. a => a + 1 将接收 43 作为待改变的状态，然后返回 44 作为下一次的状态。
3. a => a + 1 将接收 44 作为待改变的状态，然后返回 45 作为下一次的状态。
4. 没有其它队列需要更新, 最后 React 将会存储 45 作为最后的状态。

> 在开发模式下，React 可能会调用两次你的更新函数，目的是确保他们是纯的没有副作用。

## 理解 useRef、useMemo、useCallback

useRef 可用来存储一个引用值（不会受 re-render 影响），也可用来获取 dom 节点。

useMemo 用来缓存一个值。当依赖项为空数组时，缓存值永远不会变。当有依赖性时，每次 re-render 如果依赖改变，那么将重新执行函数，将新的函数返回值作为缓存的数据。

useCallback 是 useMemo 的语法糖，相当于返回一个函数。

```js
  const fn1 = useCallback(() => {
    console.log(123);
  }, [])

  const fn2 = useMemo(() => () => {
    console.log(123);
  }, [])
```

## react 不会缓存组件状态的解决方案

react 不像 vue 能使用 keep-alive。

当 react 跳到另外一个页面，再返回到上一个页面。上一个页面会重新渲染。

由于水平有限，所以决定将接口数据进行缓存。第一次没有缓存会请求。第二次执行方法时判断是否有缓存，有就直接返回缓存的数据。

我觉得要理解这个 hook，要明白模块只会被加载一次。还要明白每次 re-render 有些方法是会被重新执行，有些方法是会被重新定义。

useCache hook

```jsx
interface Cb {
  (...arg: unknown[]): unknown
}
const cacheMap = new Map();

export default (key: string, callback: Cb) => {
  return async (cache = true) => {
    const result = cacheMap.get(key)

    if(cache && result) {
      return result
    }

    const res = await callback()
    cacheMap.set(key, res)

    return res
  }
}
```

使用

```jsx
function App () {  

  const [count, setCount] = useState(0)

  useEffect(() => {
    fn()
  })

  async function fn () {
    const res = await getStatus() 
    console.log('===>', res);
  }

  const getStatus = useCache('getStatus', () => {
    const arr = [1,2,3,4]
    const res = [] as number[]
    arr.forEach(num => {
      console.log(num);
      res.push(num)
    });

    return res
  })

  function add () {
    setCount(c => c + 1)
  }

  return (
    <>
      <h2>==</h2>
      <br />
      {count}
      <br />
      <button onClick={add}>add</button>
      <h2>==</h2>
    </>
  )
}
```

## 定义组件 props 类型，并且声明默认值

### 普通情况

使用 defaultProps。

```jsx
import React, { memo } from "react"

interface Props {
  name?: string
}

// function Index(props: Props){
//   return (
//     <button>name: {props.name}</button>
//   )
// }

const Index = (props: Props) => {
  return (
    <button>name: {props.name}</button>
  )
}

// const 的作用域不会提升
Index.defaultProps = {
  name: '小王'
} as Props

export default memo(Index)
```

### 复杂情况：当使用了 forwardRef

forwardRef 用来获取子组件内的 dom 元素。

如果使用了 forwardRef，那么定义默认值需要这么写：

```jsx
import React, {forwardRef} from 'react'

interface Props {
  btnClick?: (item: any) => void
  totalScore?: number
}

const Index = forwardRef(function HeadReward(props: Props, ref: any) {
    // ...
})

Index.defaultProps = {
  btnClick: () => {
    //
  },
  totalScore: 0
} as Props

export default Index
```

## React 性能优化注意事项

由于全部使用的函数式组件，每次 setState 的时候，都会导致 re-render。

而 React 是父组件更新，子组件也会跟着更新，所以需要做一些优化。

### 1 子组件使用 memo 包裹

父组件

```jsx
function App() {
  const [count, setCount] = useState(0)

  return (
    <>  
      <div>{count}</div>
      <button onClick={() => setCount(count + 1)}>add</button>
      <Foo />
    </>
  )
}
```

子组件

```jsx
import { memo } from "react"

const Index = () => {
  console.log('re-render');
  
  return <div>子组件</div>
}

export default memo(Index)
```

### 2 传递给子组件的函数使用 useCallback 包裹

如果不传递给子组件，可以不使用 useCallback 包裹。因为 re-render 时只是重新定义了一遍，函数内部并没有执行。

为什么要用 useCallback 包裹，因为 re-render 时前后创建的两个函数引用地址并不一样，Object.is 比较是否相同时会返回 false。

父组件

```jsx
import { useCallback, useState } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {
  const request = useCallback(() => {
    setTimeout(() => {
      console.log('请求');
    }, 10)
  }, [])

  // const request = () => {
  //   setTimeout(() => {
  //     console.log('请求');
  //   }, 10)
  // }

  const [count, setCount] = useState(0)

  return (
    <> 
      <button onClick={()=>setCount(count + 1)}>{count}</button>
      <Foo request={request} />
    </>
  )
}

export default App
```

子组件

```jsx
import { memo } from "react"

interface Props {
  request: () => void
}

const Index = (props: Props) => {
  console.log('re-render');
  
  return (
    <button onClick={props.request}>按钮</button>
  )
}

export default memo(Index)
```

### 3 传递给子组件的引用数据类型需要使用 useMemo 包裹。其实和上面说的传递函数给组件，函数要用 useCallback 包裹起来是同一个道理。函数也是引用数据类型，是一个特殊的对象！

父组件

```jsx
import { useCallback, useMemo, useState } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {
  const obj = useMemo(() => ({
    name: 'xiaowang',
    age: 19
  }), [])

  const [count, setCount] = useState(0)

  return (
    <> 
      <button onClick={()=>setCount(count + 1)}>{count}</button>
      <Foo obj={obj}/>
    </>
  )
}

export default App
```

子组件

```jsx
import { memo } from "react"

interface Props {
  obj: {
    name: string,
    age: number
  }
}
const Index = (props: Props) => {
  console.log('re-render');
  
  return (
    <button>按钮</button>
  )
}

export default memo(Index)
```

### 4 hook 使用 useMemo 包裹

为什么 hook 要使用 useMemo 包裹？因为我们使用 hook 本质上是在调用一个函数执行的计算结果。

每次 re-render 的时候，都去执行一下 hook 函数的话，可能会造成性能上的损失。所以可以使用 useMemo 将函数执行的计算结果缓存起来。

组件

```jsx
function App() {
  const list = usePow([1,2,3,4])
  console.log(list);

  const list2 = usePow([1,2])
  console.log(list2);
  
  
  const [count, setCount] = useState(0)

  return (
    <> 
      <button onClick={()=>setCount(count + 1)}>{count}</button>
      <Foo />
    </>
  )
}
```

hook

```jsx
import { useMemo } from 'react';

export default (list: number[]) => {
  return useMemo(() => list.map(num => {
    console.log('hook 执行了');
    return Math.pow(num, 2)
  }), [])
}
```

## 在函数式组件中使用 ref

### 获取当前组件内的 dom 节点

```jsx
import { useRef, useEffect, useState } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {  
  const button = useRef(null)

  useEffect(() => {
    console.log(button.current);
  }, [])

  return (
    <>
      <h2>==</h2>
      <button ref={button} >按钮</button>
      <h2>==</h2>
    </>
  )
}

export default App
```

### 获取子组件的 dom 节点

使用 forwardRef 转发。

父组件

```jsx
import { useRef, useEffect } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {  
  const button = useRef(null)

  useEffect(() => {
    console.log(button.current);
  }, [])

  return (
    <>
      <h2>==</h2>
      <Foo ref={button} />
      <h2>==</h2>
    </>
  )
}

export default App
```

子组件

```jsx
import React, { forwardRef } from "react"

interface Props {
  name?: string
}

const Index = (props: Props, ref: any) => {
  return (
    <button ref={ref}>name: {props.name}</button>
  )
}

export default forwardRef(Index)
```

### memo 配合 forwardRef 一起使用

用 memo 包裹 forwardRef 转发后的组件，防止子组件不必要的 re-render。

```jsx
import React, { forwardRef, memo } from "react"

interface Props {
  name?: string
}

const Index = (props: Props, ref: any) => {
  // console.log(123);
  
  return (
    <button ref={ref}>name: {props.name}</button>
  )
}

export default memo(forwardRef(Index))
```

### memo 配合 forwardRef 和 defaultProps 一起使用

在 forwardRef 转发后返回的组件中添加 defaultProps 属性。以便当组件没有接收到父组件传来的 prop，使用默认值。

注意这里只能在 forwardRef 转发后返回的组件中添加 defaultProps 属性，否则会报错。

```jsx
import React, { forwardRef, memo } from "react"

interface Props {
  name?: string
}

const Index = forwardRef((props: Props, ref: any) => {
  
  return (
    <button ref={ref}>name: {props.name}</button>
  )
})
// 注意这里只能在 forwardRef 转发后返回的组件中添加 defaultProps 属性
// 否则会报错。
Index.defaultProps = {
  name: '小王'
} as Props

export default memo(Index)
```

### 获取循环列表的 dom 节点

#### 列表在当前组件内

给 ref 传入一个 函数

```jsx
import { useRef, useEffect, useState } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {  
 const buttonList = useRef<Element[]>([])

  function getRef(dom: Element | null) {
    if(!dom) return

    buttonList.current.push(dom)
  }
  return (
    <>
      {
      [1,2,3,4,5].map((num) => {
        return <button ref={getRef} key={num}>name: {num}</button>
      })
      }
    </>
  )
}

export default App
```

#### 列表在子组件内

父组件

```jsx
import { useRef, useEffect, useState } from 'react'
import Foo from '@/components/foo'
import './App.css'

function App() {  
  const buttonList = useRef(null)

  useEffect(() => {
    console.log(buttonList.current);
  }, [])

  return (
    <>
      <h2>==</h2>
      <Foo ref={buttonList} />
      <h2>==</h2>
    </>
  )
}

export default App
```

子组件通过 useImperativeHandle 自定义向父组件暴露属性、方法等。

```jsx
import React, { forwardRef, memo, useRef, useImperativeHandle } from "react"

interface Props {
  name?: string
}

const Index = forwardRef((props: Props, ref: any) => {

  const buttonList = [] as (Element | null)[]

  function getRef(dom: Element | null) {
    if(!dom) return

    buttonList.push(dom)
  }

  useImperativeHandle(ref, () => ({
    buttonList
  }))

  return (
    <>
      {
      [1,2,3,4,5].map((item) => {
        return <button ref={getRef} key={item}>name: {props.name}</button>
      })
      }
    </>
  )
})

Index.defaultProps = {
  name: '小王'
} as Props

export default memo(Index)
```

## 禁止用户手动硕放屏幕

在 index.html 添加如下脚本

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" />
```

## react 样式隔离

目前还没有一个感觉很好的解决方案。所以只能约定命名规范，每个页面或组件的根标签的 css 类名都不能一样。

## px 转 rem，实现页面自适应宽度

使用 amfe-flexible、postcss，将 px 转为 rem。使用 ui 库 antd-mobile。

```shell
pnpm add postcss-pxtorem postcss amfe-flexible -S
```

在入口文件处引入

```tsx
// 可伸缩布局库
import 'amfe-flexible'
```

在根目录创建 postcss.config.cjs

```tsx
// eslint-disable-next-line no-undef
module.exports = {
  plugins: {
    'postcss-pxtorem': {
      rootValue: 37.5, // 设计稿元素尺寸/10
      unitPrecision: 5, 
      propList: ['*'], // 是一个存储哪些将被转换的属性列表，这里设置为['*']全部，假设需要仅对边框进行设置，可以写['*', '!border*']
      selectorBlackList: [], // 则是一个对css选择器进行过滤的数组，比如你设置为['el-']，那所有el-类名里面有关px的样式将不被转换，这里也支持正则写法。
      replace: true,
      mediaQuery: false, // 媒体查询( @media screen 之类的)中不生效
      minPixelValue: 0, // px 绝对值小于 0 的不会被转换
      exclude: /node_modules/i,
    },
  },
}
```

## 环境变量和模式

查看文档了解更多 ：

https://cn.vitejs.dev/guide/env-and-mode.html

命令行通过 --mode 选项重写模式。

```tsx
  "scripts": {
    "dev:test": "vite --mode test",
    "dev:pre": "vite --mode pre",
    "dev:prod": "vite --mode prod",
    "build:test": "tsc && vite build --mode test",
    "build:pre": "tsc && vite build --mode pre",
    "build:prod": "tsc && vite build --mode prod"
  },
```

> tsc：编译 TypeScript

vite 会通过 dotenv 读取项目中的下列文件，以添加额外的环境变量。

```tsx
# 必须以 VITE_ 开头，否则为 undefined

# .env.test
VITE_SHAREURL='https://test.share.com.cn'
VITE_BASEURL='https://www.test.baidu.com'

# .env.pre
VITE_SHAREURL='https://pre.share.com.cn'
VITE_BASEURL='https://www.pre.baidu.com'

# .env.prod
VITE_SHAREURL='https://prod.share.com.cn'
VITE_BASEURL='https://www.prod.baidu.com'
```

这样我们就可以根据环境来选择操作了。比如只有在开发模式下引入在线调试功能。

```tsx
if (import.meta.env.MODE === 'test') {
  // eslint-disable-next-line @typescript-eslint/ban-ts-comment
  // @ts-ignore
  import('https://cdn.staticfile.org/vConsole/3.3.4/vconsole.min.js').then(() => {
    // eslint-disable-next-line @typescript-eslint/ban-ts-comment
    // @ts-ignore
    new window.VConsole();
  })
}
```

在生产环境下去除 log 日志和 debugger。

defineConfig 可以传入一个函数，通过 config 参数可以获取到模式 mode 的值。

https://cn.vitejs.dev/config/#conditional-confi

```tsx
export default defineConfig((config) => {
  return {
    build: {
      minify: 'terser',
      terserOptions: {
        compress: {
          // 默认是 false。移除 console
          drop_console: config.mode === 'prod',
          // 默认是 true。移除 debugger
          drop_debugger: config.mode === 'prod',
        },
      }
    }
  }
})
```

## 打包开启 gzip 压缩

gzip 压缩可以大大节省服务器的网络带宽。

啥是 gzip ？我理解的就是一个能压缩内容的工具。浏览器请求 url时，如果在 request header 中设置属性 accept-encoding: gzip，则表明浏览器支持 gzip 压缩。

服务器收到浏览器发送的请求之后，判断浏览器是否支持 gzip。如果支持 gzip，则向浏览器传送 gzip 压缩过的内容，不支持则向浏览器发送未经压缩的内容。一般情况下，浏览器和服务器都支持 gzip。

浏览器接收到服务器的响应之后判断内容是否被压缩，如果被压缩则解压缩后才显示页面内容。

如何在 vite 项目中开启 gzip 压缩呢？首先下载插件：

```shell
pnpm add rollup-plugin-gzip --save-dev
```

在 vite.config.ts 中

```jsx
import { defineConfig } from 'vite'
import gzipPlugin from 'rollup-plugin-gzip'

export default defineConfig({
  base: './',
  build: {
    rollupOptions: {
      plugins: [gzipPlugin()]
    }
  }
})
```

接下来，打包后会发现多了个 .gz 文件，说明开启 gzip 压缩成功。

### 开发时，低版本浏览器不识别 es6 新语法问题

> 前提： 在用 vite 创建项目的时候，不要选用 SWC。SWC 不支持下面的配置。
>
> vue 项目可以参考：
>
> https://www.cnblogs.com/ygunoil/p/15728516.html

vite.config.ts 里可以配置 babel 插件。

```js
  plugins: [react({
   babel: {
     plugins: [
       "@babel/plugin-proposal-optional-chaining",
       "@babel/plugin-proposal-nullish-coalescing-operator"
     ]
   }
 })],
```

我们直接在根目录下的 .babelrc 里进行配置。使用 babel 的预设 preset-env，免得一个个配插件。

```js
plugins: [
 react({
   babel: {
       // 表示使用根目录下 .babelrc 文件的 babel 配置
       babelrc: true,
   },
 })
],
```

.babelrc

```js
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "modules": false
      }
    ]
  ],
  "targets": "chrome 70"
}
```

配置好后 vite 就可以使用可选链和空值合并运算符等 es6 新语法了。

### 配置 alias 时，提示找不到 path

在 ts + vite 项目里提示 “path” 找不到。

原因分析：path 模块是 node.js 内置的功能，但是 node.js 本身并不支持 typescript，所以直接在 typescript 项目里使用是不行的。

解决办法

```nginx
npm install @types/node --save-dev
```

![图片](/images/2024/09/08/640.png)

这样我们就方便配置 alias 了。因为它的路径必须是绝对路径。

```js
export default defineConfig({ 
  resolve: {
    alias: {
      // 只能是绝对路径
      // '@': fileURLToPath(new URL('./src', import.meta.url)),
      '@': path.resolve('./src')
    },
  },
})
```

### 让打包后的代码支持低版本浏览器

vite 开发和打包的构建逻辑是不一样的。默认打包的目标是支持原生 ESM script 标签、支持原生 ESM 动态导入和 import.meta的浏览器：

- Chrome >=87
- Firefox >=78
- Safari >=14
- Edge >=88

为了让打包后的代码支持传统浏览器，可以使用 legacy 插件。否则如果你的代码使用了可选链、控制合并运算符等 es6 新语法新特性，打包运行低版本浏览器是会报错的。

```nginx
pnpm add -D @vitejs/plugin-legacy
```

vite.config.ts

```js
plugins: [
  legacy({
    targets: ['> 0.25%', 'last 2 versions and not dead'],
  }),
],
```

## 代码之外的东西

### 常用的脚本命令

```shell
# 在当前目录下，把 文件 a 及以下文件移动到 b 目录下（改名也可以用这个命令）
mv a b

# 创建一个文件夹
mkdir test

# 创建一个文件
touch hello.md

# 查看当前目录下所有东西
ls

# 删除文件或文件夹
rm 文件名

# 删除文件及以下的文件
rm -rf xxx

# 查看 shell 全局变量
env

# 输出全局变量
echo $HOME or $HOME

# 查看当前文件目录
pwd

# 清空命令行窗口
clear

# printenv 查看某个环境变量，变量前不用加 $ 符号了
printenv HOME 

# echo 查看某个环境变量，变量前要加 $ 符号
echo $HOME 或者 echo ${HOME}

```