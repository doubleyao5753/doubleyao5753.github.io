---
title: 总结ES6常用语法
date: 2019-08-18 00:41:14
excerpt: ES6中常用的API
photos:
  [
    https://hbimg.huabanimg.com/d8d5d418b8e69873901fe5fe6bde5adb0bbbdcc73e298d-J1sCyu_fw658,
  ]
tags: ES6 JavaScript
---

ES6 是什么，就不赘述了。

如果你也跟当下的我一样，有一些 JavaScript 基础，想在工作中提高开发的效率和优化简化代码，但暂时又没有充分的时间系统的学习 ES6，那么此文就很适合你啦。或许大家都知道，只要基础够扎实，跟着阮一峰大佬的名著[《ES6 标准入门》](http://es6.ruanyifeng.com/)走一遍敲敲代码，学好并应用以来是完全不是事儿的。我们只是暂时性的尝尝鲜，亦或是学过了记不住，平常用的时候模糊了，拿来翻一翻也是再好不过的。

#### 快速搭建 ES6 环境

现如今大多数浏览器并不认识 ES6 语法，所以我们需要一些打包编译工具例如：[webpack](https://www.webpackjs.com/)、[Babel](https://www.babeljs.cn/)、[gulp](https://www.gulpjs.com.cn/) 等等。至于具体环境的搭建，可以参考这位仁兄的个人博客，非常详细，致敬！

[最贴心的 ES6 环境搭建](https://www.cnblogs.com/zhouyangla/p/7076292.html)

#### ES6 语法：常用的就这么几个

无论是 ES6 还是 English，学习任何一门语言，首要的在其语言的规则和规律 —— 语法。

别看 ES6 语法书那么厚一本，文档目录那么长一大串，真正用在刀刃上的不过十分之一。用取经取过来的话说：下面提到的语法可能也就是 es6 新特性的 10%-20%，但是开发上占了 80%左右的。所以只要我们暂时弄清楚这部分，挑最核心最实用的学习，便是用最少的时间成本获取最优的价值收益。

##### 1. 变量声明：let / const

```javascript
// No.1 块级作用域

// 用var声明的变量，可以全局使用或者在函数中的作用域链中生效
// 用let声明的变量，只能在let命令所在的 { 代码块 } 内生效

{
  var a = 0
  let b = 1
  console.log(a) // --> 0
  console.log(b) // --> 1
}
console.log(a) // --> 0
console.log(b) // --> Uncaught ReferenceError: b is not defined 意外的引用错误
```

```javascript
// ==>  if条件语句与for循环中用let替代var (常用)

for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i)
  }, 1000)
}
console.log(i)
// --> 5
// --> 内部定时器输出 5个5

for (let i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i)
  }, 1000)
}
console.log(i)
// --> Uncaught ReferenceError: i is not defined
// --> 内部定时器输出 0 1 2 3 4
```

首先需要清楚，for 循环的小括号内分为三部分：一是变量的声明；二是循环的退出条件；三是每次循环后变量要执行的操作。

- `var` 外部输出：在循环的过程中，变量 i 只会在第一次循环时被声明，在后续的变量操作以及退出循环，始终都是用的那个被声明了一次的唯一变量 i , 并且外部可以拿到里面用 var 声明的 i ，因此最后打印退出时的结果 5
- `let` 外部输出：在循环的过程中，每一次循环 let 都会创建一个块级作用域并声明变量 i ，也就是说**有多少个循环就有多少个块级作用域和变量 i** ，每次循环，变量 i 都会记住上次循环的结果并赋值给下一个循环，因此内部有 0 到 4 五个变量和对应的块级作用域，但是外部永远拿不到，打印 `引用错误`
- 内部的定时器输出：典型的 JavaScript 执行机制知识点，参考 [这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89) ，你会发现，无论用`var`还是`let`，浏览器都会率先打印出外部的`console.log`，因为定时器里面用的是回调函数，是异步执行的，并不会按照代码顺序等待代码块内部执行完毕后才执行外部的输出，这里将它牵扯进来，是为了更好地理解`let`在循环内部创建了五个变量且分别输出不同的数；而`var`永远都是那一个变量在被不断的被下一次循环所替代直到退出

```javascript
// No.2 变量提升与暂时性死区

// var: 变量提升现象，在创建时就已被初始化，并且赋值为undefined;
console.log(foo) // 输出undefined
var foo = 2

// let：也有变量提升，但在创建时不会被初始化，不会赋值为undefined，直到声明语句执行的时候才被初始化
console.log(bar) // -->报错ReferenceError
let bar = 2

let some
console.log(some) // -->undefined
```

- 初始化的时候如果使用 let 声明的变量没有赋值，则会默认赋值为`undefined`
- 使用`let/const`时，变量创建到初始化之间的片段就叫**暂时性死区**

```javascript
var a = 123

if (true) {
  a = 'abc' // -->ReferenceError
  let a
  let a // -->报错
}
```

- 如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，**从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，都会报错。**
- `let / const`不允许在相同作用域内，重复声明同一个变量

```javascript
// No.3 const 常量

// 使用const，就必须赋值
const a;      // --> SyntaxError 无法编译，报语法错误

// const 定义的叫做只读常量，禁止被重新赋值
const a = 1
a = 2 		// --> SyntaxError: "a" is read-only

// const声明的是一个引用类型时，不能改变它指向的内存地址
const mouse = {
    one: 1
}
mouse.one = 2 // --> 改变原来所指的对象 完全OK
mouse = {
    two: 2
}
// --> 直接使其指针指向另一个内存地址 SyntaxError
```

- 使用 const，就必须赋值
- const 定义只读常量，禁止被重新赋值
- const 声明的是一个引用类型时，不能改变它指向的内存地址

##### 2. 箭头函数

##### 3. 模板字符串

##### 4. 解构赋值

##### 5. 剩余/ 扩展运算符

##### 6. 简写对象属性/方法

##### 7. Promise

##### 8. Module

[掘金一万字的 ES6 语法](https://juejin.im/post/5c6234f16fb9a049a81fcca5)

[掘金 ES6 常用 API](https://juejin.im/post/5a08e5c55188252abc5dd96f#heading-8)

[思否常用语法](https://segmentfault.com/a/1190000014824675#articleHeader7)

[掘金 30 分钟搞定 ES6](https://juejin.im/entry/58f21df95c497d006c87469e)
