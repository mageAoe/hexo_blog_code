---
title: 前端面试题 - Javascript补充篇
categories: [前端,面试题,Javascript补充]
tags: [前端,面试题,Javascript补充 ]
date: 2024-09-08
---

前端面试题 - Javascript补充篇

<!--more-->

# JS补充

## 1.undefined和null有什么区别？


在了解它们之间的差异之前，我们首先要了解它们之间的相似之处。

- 它们都属于 JavaScript 的 7 种原始类型当中。

```
string、number、null、undefined、boolean、symbol、bigint
```

- 它们是虚值。使用Boolean(value) 或!!value 将其转换为布尔值时计算结果为 false的值

![图片](/images/2024/09/08/js-1-1.png)

让我们来谈谈两者的差异

- undefined是尚未分配特定值的变量的默认值。或者没有明确显式console.log(1)返回值的函数，或对象中不存在的属性。JavaScript 引擎为我们分配了undefined值。

![图片](/images/2024/09/08/js-1-2.png)

- null是 “不代表任何值的值”。 null已被明确定义为变量的值。在此示例中，当fs.readFile该方法不抛出错误时，我们得到一个null值。

![图片](/images/2024/09/08/js-1-3.png)

比较null和undefined时，我们得到了我们得到true，当我们使用==和false时，我们同时也在使用时===

![图片](/images/2024/09/08/js-1-4.png)

## 2. &&运算符是做什么的？

&&或逻辑AND 运算符在其操作数中找到第一个falsy表达式并返回它，如果未找到任何 falsy 表达式，则返回最后一个表达式。它采用短路来防止不必要的工作。在我的一个项目中关闭数据库连接时，我在catch块中使用它。
![图片](/images/2024/09/08/js-1-5.png)

使用 if 语句

![图片](/images/2024/09/08/js-1-6.png)

使用&& 运算符

![图片](/images/2024/09/08/js-1-7.png)

## 3. ||运算符是做什么的？

||或逻辑OR 运算符在其操作数中找到第一个真表达式并返回它。这也使用短路来防止不必要的工作。在支持 ES6 默认函数参数之前，它以前用于初始化函数中的默认参数值。

![图片](/images/2024/09/08/js-1-8.png)

## 4.使用 + 或一元加号运算符是将字符串转换为数字的最快方法吗？

根据 MDN文档，+是将字符串转换为数字的最快方法，因为如果它已经是一个数字，它不会对该值执行任何操作。

## 5. 什么是dom？

DOM 代表文档对象模型是HTML 和XML 文档的接口 （API）。当浏览器第一次读取我们的HTML文档时，它会创建一个大对象，一个基于HTML文档的非常大的对象，这就是DOM。它是从HTML文档建模的树状结构。DOM 用于交互和修改 DOM结构或特定的元素或节点。

想象一下，如果我们有一个这样的HTML结构。

![图片](/images/2024/09/08/js-1-9.png)
DOM 相当于是这样的。

![图片](/images/2024/09/08/js-1-10.png)

document在JavaScript 中的对象表示 DOM。它为我们提供了许多方法，我们可以用来选择元素以更新元素内容等等。

## 6.什么是事件传播？

当一个事件发生在 DOM 元素上时，该事件不会完全发生在一个元素上。在冒泡阶段，事件冒泡或传到它的父级、祖父母、祖父母的父级，直到一直到达，而在捕获阶段，windowwindow事件从下到触发事件的元素或事件目标 。

**事件传播有三个阶段。**

1）捕获阶段 – 事件从window然后向下到每个元素，直到到达目标元素。

2）目标阶段 – 事件已到达目标元素。

3）冒泡阶段 – 事件从目标元素向上冒泡，然后每个元素上升，直到到达window。

![图片](/images/2024/09/08/js-1-11.png)

## 7.什么是事件冒泡？

当一个事件发生在 DOM 元素上时，该事件不会完全发生在一个元素上。在冒泡阶段，事件冒泡，或者它去它的父，它的祖父母，它的祖父母，它的祖父母的父母，直到它一直到达window。

我们有这样的示例标记
![图片](/images/2024/09/08/js-1-12.png)

还有js代码
![图片](/images/2024/09/08/js-1-13.png)

addEventListener方法有第三个可选参数（默认为 false）用户捕获false带有默认值的事件将发生在冒泡阶段，如果true发生在事件捕获阶段，如果我们单击它记录的child元素和分别在window document html grandparent parent child上，这是事件冒泡

## 8.什么是事件捕获？

当一个事件发生在 DOM 元素上时，该事件不会完全发生在一个元素上。在捕获阶段中，事件从一直到触发事件的元素开始。

如果我们有这样的示例标记。

![图片](/images/2024/09/08/js-1-14.png)

还有我们的js代码。
![图片](/images/2024/09/08/js-1-15.png)

该方法具有第三个可选参数 useCapture，如果事件将在捕获阶段发生，则事件的默认值将在冒泡阶段发生。如果我们单击该元素，它会记录控制台上的元素。这是事件捕获。

## 9.event.preventDefault()event.stopPropagation()方法有什么区别？

event.preventDefault()方法防止元素的默认行为。如果在元素中使用它可以防止form它从提交。如果在元素中使用它可以防止anchor它从导航。如果用在一个它可以防止contextmenuevent.stopPropagation()它从显示或展示。当方法停止传播事件或阻止事件发生在冒泡或捕获阶段

## 10.如何知道元素中是否使用event.preventDefault()方法？

我们可以在事件对象中使用event.defaultPrevented属性。它返回一个boolean，指示是否在特定元素中调用event.preventDefault()。

## 11.为什么obj.someprop.x会抛出错误？

![图片](/images/2024/09/08/js-1-16.png)

显然，由于我们试图访问xsomeprop属性中具有undefined值的属性。记住，本身不存在的对象中的属性，其原型的默认值为undefined，undefined属性就是没有属性。

## 12、什么是event.target？

最简单的术语来说，event.target是发生事件的元素或触发事件的元素。

HTML标记示例

![图片](/images/2024/09/08/js-1-17.png)

示例JavaScript

![图片](/images/2024/09/08/js-1-18.png)

如果你单击按钮，它将记录按钮标记，即使我们在最外层的div上附加事件，它也会始终记录按钮，以便我们可以得出结论，event.target是触发事件的元素。

## 13．什么是event.currentTarget？

event.currentTarget是我们显式附加事件处理程序的元素。

HTML标记示例
![图片](/images/2024/09/08/js-1-19.png)
稍微改变我们的JS
![图片](/images/2024/09/08/js-1-20.png)

如果你单击按钮，即使我们单击按钮，它也会记录最外层的标记。在本例中，我们可以得出结论，event.currentTarget是我们附加事件处理程序的元素。

## 14.==和===有什么区别？

==（抽象等式）和===（严格等式）的区别在于，==在强制后按值进行比较，===按值和类型进行比较，而不受coercion
让我们更深入地挖掘==。所以首先让我们谈谈coercion。



coercion是将一个值转换为另一种类型的过程。在这种情况下，==确实隐含了coercion。在比较这两个值之前，==有一些条件需要执行。

假设我们必须比较x == y值。



如果x和y有相同的类型。然后将它们与===运算符进行比较。

如果x为null，yundefined，则返回true。

如果xundefined，并且y为null，则返回true。

如果x是类型number，而y是类型string，则返回x == toNumber(y)

如果x是类型string，y是类型number，那么返回toNumber(x) == y。

如果x是boolean类型，则返回toNumber(x) == y。

如果y是boolean类型，则返回x == toNumber(y)

如果x是string，symbol或numbery是类型object，则返回x == toPrimitive(y)

如果x是任一object，x是任一string，symbol，则返回toPrimitive(x) == y。

返回false。

注意：toPrimitive首先使用valueOf方法，然后在对象中使用toString方法来获取该对象的原始值。
![图片](/images/2024/09/08/js-1-21.png)

这些示例都返回true。

第一个例子是条件一，因为x和y具有相同的类型和值。

第二个例子是条件四y比较之前转换为number。

第三个例子是条件二。

第四个例子是条件7，因为y是boolean。

第五个例子是条件八。数组使用返回1,2的toString()方法转换为string。

最后一个例子是条件十。使用toString()方法将对象转换为返回[object Object]的string
![图片](/images/2024/09/08/js-1-22.png)


## 15.为什么在JS中比较两个相似的对象时回 false？

假设我们下面有一个例子。
![图片](/images/2024/09/08/js-1-23.png)

JavaScript对对象和原始值的比较不同。在原始值中，它们按值比较，而在对象中，它通过引用或存储变量的内存中的地址来比较它们。这就是为什么first console console.log语句返回false，第二个console.log语句返回true。a和c具有相同的引用，而a和b则不返回true。

## 16. 双非运算符是做什么的？

双非运算符将右侧的值强制转换为布尔值。基本上，这是一种将值转换为布尔值的花哨方式。

![图片](/images/2024/09/08/js-1-24.png)

## 17.如何在一行中计算多个表达式？

我们可以使用逗号运算符在一行中计算多个表达式。它从左到右计算，并返回右侧或最后一个操作数最后一个项目的值
![图片](/images/2024/09/08/js-1-25.png)
如果你记录x的值，那就是27。首先，我们将x的值增加为6，然后我们调用函数addFive(6)，传递6作为参数，并将结果分配给x，x的新值为11。之后，我们将x的当前值乘以2，并将其分配给x，更新的x值为22。然后，我们将x的当前值减去5，并将结果分配给x，更新后的值为17。最后，我们将x的值增加10，并将更新的值分配给x，现在x的值将是27

## 18.什么是Hoisting（提升）？

Hoisting是一个术语，用于描述变量和函数移动到其（全局或函数）范围的顶部，我们在哪里定义该变量或函数。

好的，为了理解Hoisting，我必须解释执行上下文。

执行上下文是当前正在执行的“代码环境”。执行上下文有两个阶段编译和执行。

编译-在此阶段，它获取所有函数声明，并将其提升到其范围的顶部，以便我们稍后可以引用它们，并获得所有变量声明（用var关键字声明），并提升它们，并给它们一个未定义的默认值。

执行-在此阶段，它将值分配给之前提升的变量，并执行或调用函数（对象中的方法）。

注意：只有使用var关键字声明的函数声明和变量才被保留，而不是函数表达式或arrow函数，let和const关键字。

好的，假设我们在下面的全局范围内有一个示例代码。

![图片](/images/2024/09/08/js-1-26.png)
此代码分别记录undefined，1，Hello Mark!

因此，编译阶段如下所示。

![图片](/images/2024/09/08/js-1-27.png)

例如，我改变了变量和函数调用的分配。

编译阶段完成后，它将开始执行阶段，调用方法并将值分配给变量。

![图片](/images/2024/09/08/js-1-28.png)

## 19.什么是范围？

JavaScript中的范围是我们可以有效访问变量或函数的区域。JavaScript有三种类型的作用域。全局作用域、函数作用域和块作用域（ES6）。

- 全局作用域 - 在全局命名空间中声明的变量或函数位于全局作用域中，因此可以在代码中的任何地方访问

![图片](/images/2024/09/08/js-1-29.png)

- 函数范围 - 函数内声明的变量、函数和参数可以在函数内部访问，但不能在函数外部访问。

![图片](/images/2024/09/08/js-1-30.png)
块作用域 - 在块{}内声明的变量（let，const）只能在块内访问。
![图片](/images/2024/09/08/js-1-31.png)

范围也是一组用于查找变量的规则。如果当前范围内不存在一个变量，它会查找并在外部作用域中搜索一个变量，如果再次不存在，它会再次查找，直到到达全局范围，如果变量存在，那么我们可以使用它，如果不存在，它会抛出错误。它搜索最近的变量，一旦找到它，它就会停止搜索或查找。这称为作用域链
![图片](/images/2024/09/08/js-1-32.png)

## 20.什么是 Closures（闭包）

Closures只是函数在声明时记住变量和参数在其当前作用域、父函数作用域、父函数作用域上的引用的能力，直到它借助作用域链到达全局作用域。基本上，它是声明函数时创建的 Scope

示例是解释 Closures的好方法。
![图片](/images/2024/09/08/js-1-33.png)
在这个例子中，当我们声明函数时，全局范围是Closures的一部分。

变量globalVar在图像中没有值的原因，因为该变量的值可能会根据我们调用函数的位置和时间而变化。

但在我们上面的例子中，globalVar变量的值为abc。

让我们举一个复杂的例子。

![图片](/images/2024/09/08/js-1-34.png)

对此的解释是，当我们调用outerFunc函数并将返回的值innerFunc函数分配给变量x时，outerParam将具有外部值，即使我们为outerVar变量分配一个新的值outer-2，因为重新分配发生在调用outer函数之后，在那个时候，当我们调用outerFunc函数时，它会在范围链中查找outerVar的值，outerVar的值将为“outer”。现在，当我们调用引用innerFunc的x变量时，innerParam将有一个内部值，因为这是我们在调用中传递的值，globalVar变量将具有猜测值，因为在调用x变量之前，我们将为globalVar分配一个新值，在调用x时，范围链中globalVar的值是猜测。



我们有一个例子，表明存在不正确理解Closures的问题。

![图片](/images/2024/09/08/js-1-35.png)

由于Closures，此代码无法正常工作。

var关键字构成全局变量，当我们推送函数时我们返回全局变量i。因此，当我们在循环后调用该数组中的其中一个函数时，它会记录5，因为我们得到i的当前值是5，我们可以访问它，因为它是一个全局变量。因为闭包会保留该变量的引用，而不是创建时的值。我们可以使用IIFIES或更改var关键字以进行块范围界定来解决这个问题

## 22. JS由那三部分组成

1. ECMAScript
2. 文档对象模型（DOM）
3. 浏览器对象模型（BOM）： 对浏览器窗口进行访问和操作

## 23. JS有那些内置对象

String Boolean Number Array Object Function Math Date RegExp...

- 常用的：
  - Math
  - Date
  - Array
  - String

## 24. 操作数组的方法有哪些

push 、 pop 、 sort 、 splice、 unshift 、 shift 、reverse 、 concat 、 join、map、filter、every、some、forEach、reduce、isArray、findIndex、find、includes、slice

哪些方法会改变原数组？
push、pop、unshift、shift、sort、reverse、splice

## 25. JS数据类的检测方式有哪些

typeof() : 对于基本数据类型没问题，遇到引用数据类型就不管用
instanceof() ： 只能判断引用数据类型，不能判断基本数据类型

```js
console.log([] instanceof Array);// true
console.log('abc' instanceof String); // false
```

constructor : 几乎可以判断基本数据类型和引用数据类型；如果声明了一个构造函数，并把它的原型指向了Array

```js
console.log(('abc').constructor === String); // true

 let arr = []
 console.log(arr.constructor === Array) // true
 console.log(arr.constructor === Object) // false

 let arr = []
 console.log(arr.constructor === Array); // true
 Array.prototype.constructor = 'a' 
 console.log(arr.constructor === Array); // false
```

Object.prototype.toString.call ： 可以完美解决上面的问题

## 26. 说一下闭包，闭包有什么特点

什么是闭包？ 函数嵌套函数，内部函数被外部函数返回并保存下来时，就会产生闭包
特点： 可以重复利用变量，并且这个变量不会污染全局的一种机制；这个变量是一直保存再内存中，不会被垃圾回收机制回收
缺点：闭包较多的时候，会消耗内存，导致页面的性能下降，再IE浏览器中才会导致内存泄漏
使用场景： 防抖、节流，函数嵌套函数避免全局污染的时候

## 27. 前端的内存泄露有什么理解

JS里已经分配内存地址的对象，但是由于长时间没有释放或者没办法清除，造成长期占用内存的现象，会让内存资源大幅浪费，最终导致运行速度慢，甚至崩溃的情况

垃圾回收机制
因素： 一些未声明直接赋值的变量；一些未清空的定时器；过度的闭包；一些引用元素没有清除 

## 28. 事件委托是什么

又叫事件代理，原理就是利用事件冒泡的机制来实现，也就是说把子元素的事件绑定到了父元素身上
如果子元素阻止了事件冒泡，那么委托也就不成立了
阻止事件冒泡： event.stopPropagation()
addEVentListener('click',函数名,true/false) 默认值false（事件冒泡），true（事件捕获）
好处： 提高性能，减少事件的绑定，也就减少了内存的占用（react的事件就是 事件委托，绑定再root身上）

## 29. 基本数据类型和引用数据类型的区别

* 基本数据类型：String、number、Boolean、null、undefined
  * 基本数据类型保存再栈内存中，保存的就是一个具体的值
* 引用数据类型（复杂数据类型）：Object、Function、Array
  *  保存在堆内存中，声明一个引用类型的变量，它保存的是引用类型数据的地址
  *  假如声明两个引用类型同时指向了一个地址的时候，修改其中一个那么另一个也会改变

## 30. 说一下原型链

原型就是一个普通对象，它是为构造函数的实例共享属性和方法；所有实例中引用的原型都是同一个对象
使用prototype可以把方法挂在原型上，内存只保存一份

__proto__ 可以理解为指针，实例对象中的属性，指向了构造函数的原型（prototype）
一个实例对象在调用属性和方法的时候，会依次从实例本身、构造函数原型、原型的原型上去查找

## 31. new 操作符具体做了什么

1. 先创建一个空对象
2. 把空对象和构造函数通过原型链进行链接
3. 把构造函数的this绑定到新的空对象身上
4. 根据构造函数返回的类型判断，如果是值类型，则返回对象，如果是引用类型，就要返回这个引用类型

```js
function newFun(fun,...args){
        // 1. 先创建一个空对象
        let newObj = {}
        // 2. 把空对象和构造函数通过原型链进行链接
        newObj.__proto__ = fun.prototype
        // 3. 把构造函数的this绑定到新的空对象身上
        const result = fun.apply(newObj, args)
        // 4. 根据构造函数返回的类型判断，如果是值类型，则返回对象，如果是引用类型，就要返回这个引用类型
        return result instanceof Object? result : newObj
    }

    function Person(name){
        this.name = name
    }
    Person.prototype.say = function(){
        console.log('say');
    }
    const son = newFun(Person, '李四')
    console.log(son); // Person {name: '李四'}
    console.log(son.say()); // say
```

## 32. JS是如何实现继承的

1. 原型链继承
   - 让一个构造函数的原型是另一个类型的实例，那么这个构造函数new 出来的实例就具有该实例的属性
     优点： 写法方便简洁，容易理解
     缺点： 对象共享所有继承的属性和方法。无法向父类构造函数传参

```js
 function Parent(){
        this.isShow = true
        this.info = {
            name: 'abc',
            age: 19
        }
    }
    function Child(){}
    Child.prototype = new Parent()

    const child1 = new Child()
    child1.info.gender = '男'
    console.log(child1.isShow); // true
    console.log(child1.info); // {name: 'child1', age: 19}

    const child2 = new Child()
    child2.info.age = 22
    console.log(child2.info.gender); // 男
    console.log(child2.info); // {name: 'child1', age: 19}
```

2. 借用构造函数继承
   - 在子类型构造函数的内部调用父类型构造函数；使用apply() or call() 方法将父对象的构造函数绑定在子对象上
     优点： 解决了原型链实现继承的不能传参的问题和父类的原型共享的问题
     缺点： 借用构造函数的缺点是方法都在构造函数中定义，因此无法实现函数复用。
           在父类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式

```js
    function Parent(gender){
        this.isShow = true
        this.info = {
            name: 'abc',
            age: 19,
            gender: gender
        }
    }
    function Child(gender){
        Parent.call(this, gender)
    }

    const child1 = new Child('男')
    child1.info.nextName = 'xxxx'
    console.log(child1.info); // {name: 'abc', age: 19, gender: '男', nextName: 'xxxx'}

    const child2 = new Child('女')
    console.log(child2.info); // {name: 'abc', age: 19, gender: '女'}
```

3. 组合式继承
   - 将原型链 和 借用构造函数 的组合到块。使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能保证每个实例都有自己的属性
     优点： 就是解决了原型链继承和借用构造函数继承造成的影响
     缺点： 是无论什么情况下，都会调用两次构造函数： 一次是在创建子类原型的时候，另一次是在子类构造函数内部

```js
 function Parent(gender){
        console.log('调用一次');
        this.isShow = true
        this.info = {
            name: 'abc',
            age: 19,
            gender: gender
        }
    }
    Parent.prototype.print = function(){ // 使用原型链继承原型上的属性和方法
        console.log(this.info.gender,this.info.name);
    }
    function Child(gender){
        Parent.call(this, gender) // 使用构造函数传递参数
    }
    Child.prototype = new Parent()

    const child1 = new Child('男')
    child1.info.nextName = 'xxxx'
    console.log(child1.info); // {name: 'abc', age: 19, gender: '男', nextName: 'xxxx'}
    child1.print() // 男 abc

    const child2 = new Child('女')
    console.log(child2.info); // {name: 'abc', age: 19, gender: '女'}
    child2.print() // 女 abc
```

4. ES6的class类继承
   - class 通过extends 关键字实现继承，其实质是先创造出父类的this对象，然后用子类的构造函数修改this；子类的构造方法中必须调用super方法，且只有在调用了super()之后才能使用this，因为子类的this对象是继承父类的this对象，然后对其进行加工，而super方法表示的是父类的构造函数，用来新建父类的this对象
     优点：语法简单易懂，操作方便
     缺点： 并不是所有的浏览器都支持class关键字

```js
class Animal{
        constructor(kind){
            this.kind = kind
        }
        getKind(){
            return this.kind
        }
    }

    class Cat extends Animal{
        constructor(name) {
            super('Cat')
            this.name = name
        }
        getCatInfo(){
            console.log(this.name + ':' + super.getKind());
        }
    }

    const cat = new Cat('kind')
    cat.getCatInfo() // kind:Cat
```

## 33. JS的设计原理是什么

V8引擎
运行上下文： 浏览器里面可以调用的api（DOM，window）、任务队列
调用栈： 单线程
事件循环
回调

## 34. JS种关于this指向的问题

1. 全局对象中的this指向： 指向的是window
2. 全局作用域或者普通函数中的this： 指向全局window
3. this永远指向最后调用它的那个对象： 在不是箭头函数的情况下
4. new 关键词改变了this的指向
5. apply，call，bind ： 可以改变this指向，不是箭头函数
6. 箭头函数中的this： 它的指向在定义的时候就已经确定了；箭头函数本身没有this，要看外层是否有函数
7. 匿名函数中的this： 永远指向window，匿名函数的执行环境具有全局性，因此this指向window

## 35. script标签里的async和defer有什么区别

当没有async和defer这两个属性的时候，浏览器会立刻加载并执行指定的脚本

有async： 加载和渲染后面元素的过程将和script的加载和执行并行进行（异步）

有defer： 加载和渲染后面元素的过程将和script的加载并行进行（异步），但是它的执行事件要等所有元素解析完成之后才会执行

defer会保证脚本的顺序，async不会
![图片](/images/2024/09/08/js-1-36.png)
蓝色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的；绿色线代表 HTML 解析

## 36. setTimeout最小执行时间是多少

> HTML5规定的内容：
> setTimeout最小执行时间是4ms
> setInterval最小执行时间是10ms

## 37. ES6 和 ES5有什么区别

JS的组成： ECMAScript BOM  DOM
ES5: ESMAScript5，2009年ECMAScript的第五次修订，ECMAScript2009
ES6：ESMAScript6，2015年ECMAScript的第六次修订，ECMAScript2016
ES6是ES5的下一个版本

## 38. ES6 的新特性有哪些

1. 新增块级作用域（let，const）
   - 不存在变量提升
   - 存在暂时性死区的问题
   - 块级作用域的内容
   - 不能在同一个作用域内重复声明
2. 新增了定义类的语法糖（class）
3. 新增了一种基本数据类型（symbol）
4. 新增了解构赋值
   - 从数组或者对象中取值，然后给变量赋值
5. 新增了函数参数的默认值
6. 给数组新增了API
7. 对象和数组新增了扩展运算符
8. Promise
   - 解决回调地狱的问题
   - 自身有all，reject，resolve，race方法
   - 原型上有then，catch
   - 把异步操作队列化
   - 三种状态： pending初始状态，fulfilled操作成功，rejected操作失败
   - async  await
     - 同步代码做异步的操作，两者必须搭配使用
     - async 表明函数内有异步操作，调用函数会返回promise
     - await 是组成async的表达式，结果是取决于它等待的内容，如果是promise那就是promise的结果，如果是普通函数就进行链式调用
     - await 后的promise如果是reject状态，那么整个async函数都会中断，后面的代码不执行
9. 新增了模块化（import ，export）
10. 新增了set和map数据结构
    - set就是不重复
    - map key的类型不受限制
11. 新增了generator
12. 新增了箭头函数
    - 不能作为构造函数使用，不能用new 关键字，箭头函数没有原型
    - 箭头函数没有arguments
    - 箭头函数不能用call，apply，bind去改变this的指向
    - this指向外层第一个函数的this  

## 39. call，apply，band三者有什么区别

- 都是改变this指向和函数的调用，call和apply的功能类似，只是传参的方法不同

- call方法传的是一个参数列表

- apply传递的是一个数组

- bind 传参后不会立刻执行，会返回一个改变了this指向的函数，这个函数还是可以传参的，bind()()

- call 方法的性能要比apply好一些，所以call用的更多一点

## 40. 用递归的时候有没有遇到什么问题

如果一个函数内可以调用函数本身，那么这个就是递归函数
特别注意： 写递归必须要有终止条件 return

## 41. 如何实现一个深拷贝

深拷贝就是完全拷贝一份新的对象，会在堆内存中开辟新的空间，拷贝的对象被修改后，原对象不受影响，主要针对的是引用数据类型

1. 扩展运算符
   - 缺点： 这个方法只能实现第一层，当有多层的时候还是浅拷贝

```js
let obj = {
        name: '张三',
        age: 19
    }
    let obj1 = {...obj}
    obj1.name = '李四'
    console.log(obj,obj1);
```

2. JSON.parse(JSON.stringify)
   - 缺点： 该方法并不会拷贝内部函数

```js
let obj = {
        name: '张三',
        age: 19,
        say(){
            console.log('say hello');
        }
    }
    let obj1 = JSON.parse(JSON.stringify(obj))
    obj1.name = '李四'
    console.log(obj,obj1);
```

3. 利用递归函数实现

```js
let obj2 = {
        name: '张三',
        age: 19,
        say(){
            console.log('say hello');
        },
        arr: [[1,2],3,4]
    }

    function cloneFn(origin,deep){
        let obj  = {}
        if(origin instanceof Array){
            obj = []
        }
        for (let key in origin){
            if(origin.hasOwnProperty(key)){
                if(!!deep){
                    let value = origin[key]
                    obj[key] = (typeof value === 'object' && value !== null)?cloneFn(value,deep):value
                }else{
                    obj[key] = origin[key]
                }
            }
        }

        return obj
    }

    let obj1 =  cloneFn(obj2,true)
    obj1.obj1.arr[0].push(100)
    console.log(obj2,obj1);
```

## 42. 说一下事件循环

JS是一个单线程的脚本语言
主线程 ， 执行栈， 任务队列 ， 宏任务 ， 微任务

主线程先执行同步任务，然后才去执行任何队列里的任务，如果在执行宏任务之前有微任务，那么要先执行微任务；全部执行完之后等待主线程的调用，调用完之后再去任务队列中查看是否有异步任务，这样一个循环往复的过程就是事件循环。

## 43. ajax是什么，怎么实现的

是创建交互式网页应用的网页开发技术
    - 再不重新加载整个网页的前提下，与服务器交换数据并更新部分内容

实现： 通过XMLhttpRequest对象向服务器发送异步请求，然后从服务器拿到数据，最后通过js操作DOM更新页面

## 44. get和post有什么区别

1. get一般是获取数据，post一般用来提交数据
2. get参数会放在url上，所以安全性比较差，post是放在body中
3. get请求刷新服务器或退回是没有影响的，post请求退回时会重新提交数据
4. get请求时会被缓存，post请求不会被缓存
5. get请求会被保存在浏览器历史记录中，post不会
6. get请求只能进行url编码，post请求支持很多（文件，表单等）

## 45. Promise的内部原理是什么？它的优缺点是什么？

Promise对象，封装了一个异步操作并且还可以获取成功或失败的结果
Promise主要就是解决 回调地狱的问题，之前如果异步任务比较多，同时他们之间有相互依赖的关系，就只能使用回调函数处理，这样就容易形成回调地狱，代码的可读性差，可维护性也很差
有三种状态： pedning初始状态 fulfilled成功状态  rejected失败状态

状态改变只要两种情况： 
    - pending -> fulfilled；pending -> rejected 一旦发生，状态就会凝固，不会再变

首先就是我们无法取消promise，一旦创建它就会立即执行，不能中途取消
如果不设置回调，promise内部抛出的错误就无法反馈到外面
若当前处于pending状态时，无法得知目前在哪个阶段
原理：
    构造一个promise实例，实例需要传递函数的参数，这个函数有两个形参，分别都是函数类型，一个是resolve，一个是reject
    promise身上才还有then方法，这个方法就是用指定状态改变时的确定操作，resolve是执行第一个函数，reject是执行第二个函数

## 46. promise和async await的区别是什么

1. 都是处理异步请求的方式
2. promise是es6，async await 是es6的语法
3. async await是基于promise实现的，他和promise都是非阻塞性的

**优缺点**：
    1. promise是返回对象我们要用then，catch方法处理和捕获异常，并且书写方式是链式，容易造成代码重叠，不好维护，async await是通过tra catch进行捕获异常
        2. async await最大的有点就是能让代码看起来像同步一样，只要遇到await就会立即返回结果，然后再执行后面的操作，promise.then() 的方式返回，会出现请求还没返回，就执行了后面的操作

## 47. 浏览器的储存方法有哪些

1. cookies：请求头自带cookies，存储量小，资源浪费，使用麻烦
2. localstorage ： H5加入的以键值对为标准的方式；操作方便，永久存储
3. sessionstorage ： 当前页面关闭后就会立即清理，回话级别的存储方式
4. indexedDB： H5标准的储存方式，他是以键值对进行存储，可以快速读取，适合WEB场景

## 48. 页面渲染的过程是怎么样的

DNS解析 -> 建立TCP连接 -> 发送HTTP请求 -> 服务器处理请求 -> 渲染页面（浏览器会获取HTML和CSS的资源，然后把HTML解析成DOM树，再把CSS解析成CSSOM，把DOM和CSSOM合并为渲染树，最后绘制到页面 ） ->  断开TCP连接

## 49. DOM树和渲染树有什么区别

DOM树是和HTML标签一一对应的，包括head和隐藏元素
渲染树是不包含head和隐藏元素

## 50.精灵图和base64的区别是什么

精灵图：把多张小图整合到一张大图上，利用定位的一些属性把小图显示在页面上，当访问页面可以减少请求，提高加载速度

base64： 传输8Bit字节代码的编码方式，把原本二进制形式转为64个字符的单位，最后组成字符串
base64 是会和html css一起下载到浏览器中，减少请求，减少跨域问题，但是base64转换后的体积会很大

## 51. svg格式了解多少？

基于XML语法格式的图像格式，可缩放矢量图，SVG是属于对图像形状的描述，本质是文本文件，体积小，并且不管放大多少倍都不会失真

SVG可直接插入页面中。成为DOM的一部分，然后用JS或CSS进行操作
可以作为标签使用
可作为文件被引入

## 52. 了解过JWT吗

JSON WEB TOKEN 通过JSON形式作为WEB应用中的令牌，可以在各方之间安全的把信息作为JSON对象传输

信息传输，授权

## 53. npm的底层环境是什么

node package manger，node的包管理和分发工具，已经成为分发node模块的标准，是JS的运行环境

npm的组成： 网站、注册表、命令行工具

## 54. 说一下浏览器的缓存策略

强缓存（本地缓存）、协商缓存（弱缓存）
强缓： 不发起请求，直接使用缓存里的内容，浏览器吧JS，CSS，image等存到内存中，下次用户访问直接从内存中取，提高性能

协缓： 需要向后台发请求，通过判断来决定是否使用协商缓存，如果请求内容没有变化，则返回304，浏览器就用缓存里的内容

## 55. 什么是同源策略

协议 、 子域名、 主域名 、 端口号、 资源

协议，域名，端口都要相同，一旦不同就会产生跨域

## 56. 防抖和节流是什么

都是应对页面中频繁触发事件的优化方案

防抖： 避免事件重复触发

节流：把频繁触发的事件减少，每隔一段时间执行

## 57. 什么是json？

JSON是一种纯字符串形式的数据，它本身不提供任何方法，适合在网络中进行传输
JSON数据存储在 .json文件中
也可以吧JSON数据以字符串的形式保存在数据库，cookies中

js提供了两个方法

## 58. 当数据没有请求过来的时候，怎么做？

可以在渲染数据时给一个默认值
也可以通过if判断

## 59. 有没有做过无感登录？

1. 在响应中拦截，判断token返回过期后，调用刷新token的接口
2. 后端返回过期时间，前端判断token的过期时间，去调用刷新token的接口
3. 使用定时器

## 60. 大文件上传是怎么做的？

分片上传： 
    1. 把需要上传的文件按照一定得规则，分割成相同大小的数据块
        2. 初始化一个分片上传任务，返回本次分片上传的唯一标识
        3. 按照一定得规则吧各个数据块上传
        4. 发送完成后，服务端会判断数据上传的完整性，如果完整，那么就会把数据库合并成原始文件
        断点续传： 
        服务端返回，从哪里开始  浏览器自己处理

## 61. 请尝试写出缓冲运动的效果

```js
function startMove (dom, target) {
    clearInterval(timer);
    var iSpeed = null;
    timer = setInterval(function () {
        iSpeed = (target - dom.offsetLeft) / 7; 
        iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
        if (dom.offsetLeft == target) {
            clearInterval(timer);
        }else {
            dom.style.left = dom.offsetLeft + iSpeed + 'px';
        }
    }, 30);
};

```

