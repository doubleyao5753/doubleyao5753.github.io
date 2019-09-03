---
title: 总结ES6常用语法
date: 2019-08-18 00:41:14
author: 姚尧
# excerpt: ES6中常用的API
summary: 好钢用在刀刃上，快速了解并上手ES6最常用最顺手的语法。
tags:
  - ES6
  - JavaScript
---

ES6 是什么，就不赘述了。

如果你也跟当下的我一样，有一些 JavaScript 基础，想在工作中提高开发的效率和优化简化代码，但暂时又没有充分的时间系统的学习 ES6，那么此文就很适合你啦。或许大家都知道，只要基础够扎实，跟着阮一峰大佬的名著[《ES6 标准入门》](http://es6.ruanyifeng.com/)走一遍敲敲代码，学好并应用以来是完全不是事儿的。我们只是暂时性的尝尝鲜，亦或是学过了记不住，平常用的时候模糊了，拿来翻一翻也是再好不过的。

#### 快速搭建 ES6 环境

现如今大多数浏览器并不认识 ES6 语法，所以我们需要一些打包编译工具例如：[webpack](https://www.webpackjs.com/)、[Babel](https://www.babeljs.cn/)、[gulp](https://www.gulpjs.com.cn/) 等等。至于具体环境的搭建，可以参考这位仁兄的个人博客，非常详细，致敬！

[最贴心的 ES6 环境搭建](https://www.cnblogs.com/zhouyangla/p/7076292.html)

如果你想对比编译前和编译后的代码，建议在 [Babel 官方在线实时编译器](https://babeljs.io/repl/)中编写。

#### ES6 语法：常用的真的不多

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

ES6 中对于函数最常用的扩展 —— 箭头函数 ( => )

```javascript
let oneFunction = a => a // ES6

// 编译后
var oneFunction = function oneFunction(a) {
  return a
}

;(a, b) =>
  a +
  b(
    // ES6

    // 编译后
    function(a, b) {
      return a + b
    }
  )
```

箭头之前是函数的参数，如果函数没有参数，则用一个空的圆括号代替；如果有多个函数，则将各参数写在圆括号内用逗号分隔。

```javascript
let some = m => {
  m *= 7
  alert(m)
  return m + 5
}
console.log(some(5)) // 弹框35，打印40
```

箭头之后是函数的执行体，若省略大括号，则直接返回所写语句；若执行语句多于一条，则需要使用大括号将其括起来，括号中若有返回值则需要使用 return 关键字；若箭头函数直接返回一个对象，使得对象括号与函数括号冲突，则必须在对象外面加上圆括号。

**注意事项：**

1. **函数体内的 this 对象指向定义时所在的对象，而不是调用时所在的对象。**
2. 箭头函数正因为没有自己的 this，所以不可当做构造函数，不能使用 new 命令
3. 没有 arguments 对象，如果要用则使用`剩余运算符`代替

箭头函数不仅仅只是为了简化代码而生。在 ES5 中，对于 this 的指向，我们刻骨铭心：**this 永远永远指向最后调用它的那个对象。**由此也带来许多不便，箭头函数便是对症下的药。

> 引用警句：箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层**非箭头函数的 this**，否则，this 报错为 undefined

```javascript
let name = 'LiNa'

let f1 = function() {
  console.log(name)
}

let a = {
  name: 'WangEr',
  f1: function() {
    console.log(this.name)
  },
  f2: function() {
    setTimeout(() => {
      this.f1()
    }, 1000)
  }
}

a.f1() // WangEr
a.f2() // WangEr
f1() // LiNa
// 先后打印 --> WangEr LiNa WangEr
```

`a.f1()`： 对于对象中普通的方法函数，this 指向方法所在的对象

`a.f2()`： 对于方法函数中定时器中的回调箭头函数，定时器中的回调函数属于全局异步函数，this 指向 Window，这里本应该打印`LiNa`；但是在这里使用了箭头函数后，this 指向最近一层非箭头函数的 this，因此打印`WangEr`

**建议：不要把一个对象的方法函数用作箭头函数！**

```javascript
let p = {
  n: '5753',
  arrow: () => {
    setTimeout(() => {
      console.log(this.n)
    }, 10)
  },
  common: function() {
    setTimeout(() => {
      console.log(this.n)
    }, 10)
  }
}
p.arrow() // 报错Cannot read property 'n' of undefined
p.common() // 5753
```

一旦方法函数使用了箭头函数，那么这个方法中的所有 this 都不再指向当前对象，而是往上走，这里指向了 Window。

##### 3. 模板字符串

模板字符串极大的简化了传统的字符串拼接。使用起来非常简单，核心就是一个反引号   **`**  （英文输入法下键盘左上角数字 1 左边），以及组合符号 **${  }** （美元和花括号）。

（1） 简化

```javascript
var father = document.getElementById('demo')

var obj = {
    name: 'KeyNG',
    age: 27,
    sing: function () {
        return 'Hip-Hop'
    }
}
//  ES5 字符串拼接
father.append('大家好，我的名字是' + obj.name + ',年龄' + obj.age + '岁' + ',我会唱' + obj.sing())

// ES6 模板字符串
father.append(`大家好，我的名字是${obj.name},年龄${obj.age}岁，我会唱${obj.sing()}`)
```

在一串字符串中有变量或者函数的情况下，传统的写法是使用符号` + `其拆分和拼接，无数的单引号使人抓狂。

而在ES6中，使用模板字符串，无论是普通字符串还是变量和函数，统统包裹在两个反引号中，其中再将变量和函数用`${ }`包裹一下即可，极大的简化代码，结构也一目了然，需要拼接的越多，其优势越明显。

**模板字符串的大括号内部，就是执行JavaScript代码**，因此可以放入任意的JavaScript表达式，例如进行一些简单的运算、调用函数、引用对象属性方法等等。

（2） 无需对特殊字符进行转义

```javascript
father.innerHTML += '<p><h2>你好，我叫\n\"' + obj.name + '\"\n</h2><span>只会唱唱' + obj.sing() + '</span></p>'

father.innerHTML +=
`
    <p>
    	<h2>你好，我叫 "${obj.name}" </h2>
    	<span>只会唱唱${obj.sing()}</span>
    </p>
`

```

反斜杠用来在文本字符串中插入省略号、换行符、引号和其他特殊字符。而使用模板字符串可以表示多行字符串，所有的空格和缩进都会被保留在输出之中。

##### 4. 解构赋值

ES6 允许按照一定模式，从**数组和对象**中提取值，对变量进行赋值，这就是解构赋值。

（1）数组的解构

```javascript
// 基础类型解构
let [a, b, c] = [1, 2, 3]
console.log(a, b, c) // 1, 2, 3

// 对象数组解构
let [a, b, c] = [{name: '1'}, {name: '2'}, {name: '3'}]
console.log(a, b, c) // {name: '1'}, {name: '2'}, {name: '3'}

// ...解构
let [head, ...tail] = [1, 2, 3, 4]
console.log(head, tail) // 1, [2, 3, 4]

// 嵌套解构
let [a, [b], d] = [1, [2, 3], 4]
console.log(a, b, d) // 1, 2, 4

// 解构不成功为undefined
let [a, b, c] = [1]
console.log(a, b, c) // 1, undefined, undefined

// 解构前默认赋值
let [a = 1, b = 2] = [3]
console.log(a, b) // 3, 2

```

（2）对象的解构

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；

**而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。**

```javascript
// 对象属性解构
let { f1, f2 } = { f1: 'test1', f2: 'test2' }
console.log(f1, f2) // test1, test2

// 可以不按照顺序，这是数组解构和对象解构的重要区别之一
let { f2, f1 } = { f1: 'test1', f2: 'test2' }
console.log(f1, f2) // test1, test2

// 解构对象可以重命名
let { f1: rename, f2 } = { f1: 'test1', f2: 'test2' }
console.log(rename, f2) // test1, test2

// 嵌套解构
let { f1: {f11}} = { f1: { f11: 'test11', f12: 'test12' } }
console.log(f11) // test11

// 解构对象可以定义默认值
let { f1 = 'test1', f2: rename = 'test2' } = { f1: 'current1', f2: 'current2'}
console.log(f1, rename) // current1, current2
```

（3）将解构放进函数参数中

```javascript
function func1({ x, y }) {
    return x + y
}
func1({ x: 1, y: 2}) // 3

function func1({ x = 1, y = 2 }) {
    return x + y
}
func1({x: 4}) // 6
```



##### 5. 函数默认参数和剩余参数

（1）默认参数

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function demo(x, y = 7) {
    return x + y
}

demo(3) // 10
demo(3, 9) // 12
```

（2）剩余参数

ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。你可以简单理解为，**将一个数组类型的变量作为函数的参数。**

```javascript
function add(title, ...values) {
    let sum = 0;
    for (var val of values) {
        sum += val;
    }
    return title + sum;
}

add('结果为：', 2, 5, 3) // 结果为：10  ...自动将剩余参数转为数据组

add('结果为：', [2, 5, 3]) // 在没有三个点的情况下你就得传数组
```

剩余参数可以完美而优雅的替代传统的arguments对象：

```javascript
function sortNum() {
    return Array.prototype.slice.call(arguments).sort()
}

const sortNum = (...num) => {
    return num.sort()
}
```

伪数组arguments除了length属性和索引元素之外没有任何Array属性和方法。要使用数组方法必须先转为数组。

而**剩余参数所代表的数组就是一个真正的数组**，数组特有的方法都可以使用。

另外，要注意：rest 参数之后不能再有其他参数（即**只能是最后一个参数**），否则会报错。

##### 6. 简写对象属性/方法

对象中属性重名时：

```javascript
function people(name, age) {
    return {
        name: name,
        age: age
    };
}

function people(name, age) {
    return {
        name,
        age
    };
}
```

对象中方法函数简写：

```javascript
const people = {
    name: 'lux',
    age: 32,
    getName: function() {
        console.log(this.name)
    }
    getAge () {
        console.log(this.age)
    }
}
```

##### 7. Promise



##### 8. 模块化

模块功能主要由两个命令构成：`export`和`import`。`export`导出当前模块下其它模块中可能需要使用的功能，`import`导入其它模块暴露且当前模块需要使用的功能。

一个模块就是一个独立的文件。

```javascript
--- out.js

// 部分导出
export let a = 'something'
export function fn() {
    console.log('something')
}
// 建议：使用最后打包导出前面声明的变量或函数的方式而不是每一个都加上export
export {a,b,c...}

--- in.js

// 部分导入：必须使用花括号
import {a,fn} from './out.js'
// 全部导入
import * as all from './out.js'
all.fn()
console.log(all.a)
```

`import`命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。建议凡是输入的变量，都当作完全只读，轻易不要改变它的属性。

```javascript
// 导出默认, 有且只有一个默认,不能使用花括号，导出匿名的接口
export default App
// 导入默认，没有花括号
import App from './out.js'
```

`export default`命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能唯一对应`export default`命令。

牢记：

1. 当用export default people导出时，就用 import people 导入（不带大括号）
2. 一个文件里，有且只能有一个export default。但可以有多个export。
3. 当用export name 时，就用import { name }导入（记得带上大括号）
4. 当一个文件里，既有一个export default people, 又有多个export name 或者 export
   age时，导入就用 import people, { name, age }
5. 当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example

[掘金一万字的 ES6 语法](https://juejin.im/post/5c6234f16fb9a049a81fcca5)

[掘金 ES6 常用 API](https://juejin.im/post/5a08e5c55188252abc5dd96f#heading-8)

[思否常用语法](https://segmentfault.com/a/1190000014824675#articleHeader7)

[掘金 30 分钟搞定 ES6](https://juejin.im/entry/58f21df95c497d006c87469e)