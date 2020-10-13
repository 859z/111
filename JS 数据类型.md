# 1.JS原始数据类型有哪些？引用数据类型有哪些？

* 在 JS 中，存在着 6 种种简单数据类型【原始值】，分别是：
```
Undefined
Null
Boolean
Number
String
Symbol 【Symbol（符号）是 ECMAScript 6 新增的】
```
* 引用数据类型【复杂数据类型】: 对象Object
```
包含:
普通对象-Object，
数组对象-Array，
正则对象-RegExp，
日期对象-Date，
数学函数-Math，
函数对象-Function
```

# 2.typeof 是否能正确判断类型？
* 对于原始类型来说，除了 null 都可以调用typeof显示正确的类型。
> typeof null 会输出 object，但是这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象，然而 null 表示为全零，所以将它错误的判断为 object 。虽然现在的内部类型判断代码已经改变了，但是对于这个 Bug 却是一直流传下来。
```
typeof undefined // 'undefined'
typeof null // 'object'
typeof true // 'boolean'
typeof 1 // 'number'
typeof '1' // 'string'
typeof Symbol() // 'symbol'
```
* 但对于引用数据类型，除了函数之外，都会显示"object"。

```
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```
> 注意：严格来讲，函数在 ECMAScript 中被认为是对象，并不代表一种数据类型。可是，函数也有自己特殊的属性。为此，就有必要通过 typeof 操作符来区分函数和其他对象

> typeof 是一个操作符而不是函数，所以不需要参数（但可以使用参数）

# 3. Undefined 类型
## 3.1 Undefined 类型只有一个值，就是特殊值 undefined。当使用 var 或 let 声明了变量但没有初始化时，就相当于给变量赋予了 undefined 值：

```
let message; 
console.log(message == undefined); // true
```
## 3.2 增加这个特殊值的目的就是为了正式明确空对象指针（null）和未初始化变量的区别。

> 注意，包含 undefined 值的变量跟未定义变量是有区别的。请看下面的例子：

```
let message; // 这个变量被声明了，只是值为 undefined 
// 确保没有声明过这个变量
// let age 
console.log(message); // "undefined" 
console.log(age); // 报错
```
## 3.3 【对未声明的变量，只能执行一个有用的操作，就是对它调用 typeof,返回的结果是"undefined"，】

## 3.4 undefined 是一个假值
```
let message; // 这个变量被声明了，只是值为 undefined 
// age 没有声明 
if (message) { 
 // 这个块不会执行
} 
if (!message) { 
 // 这个块会执行
} 
if (age) { 
 // 这里会报错,因为age变量没有定义
}
```

# 4. Null 类型

## 4.1 Null 类型同样只有一个值，即特殊值 null。
> 逻辑上讲，null 值表示一个空对象指针，这也是给typeof 传一个 null 会返回"object"的原因：

```
let car = null; 
console.log(typeof car); // "object"
```

## 4.2 undefined 值是由 null 值派生而来的，因此 ECMA-262 将它们定义为表面上相等，如下面的例子所示：

```
console.log(null == undefined); // true
```
## 4.2 null 是一个假值

```
let age; 
if (message) { 
 // 这个块不会执行
} 
if (!message) { 
 // 这个块会执行
} 
if (age) { 
 // 这个块不会执行
} 
if (!age) { 
 // 这个块会执行
}

```

# 5. Boolean 类型
## 5.1 Boolean（布尔值）类型是 ECMAScript 中使用最频繁的类型之一，有两个字面值：true 和 false。

## 5.2 布尔值字面量 true 和 false 是区分大小写的
> 因此 True 和 False（及其他大小混写形式）是有效的标识符，但不是布尔值

## 5.3 Boolean()转型函数
> 虽然布尔值只有两个，但所有其他 ECMAScript 类型的值都有相应布尔值的等价形式。要将一个其他类型的值转换为布尔值，可以调用特定的 Boolean()转型函数：

```
let message = "Hello world!"; 
let messageAsBoolean = Boolean(message);
console.log(messageAsBoolean) // true
```
> Boolean()转型函数可以在任意类型的数据上调用，而且始终返回一个布尔值

## 5.4 总结了不同类型与布尔值之间的转换规则
```
数据类型        转换为true的值              转换为false的值
Boolean             true                       false
String              非空字符串                  " "(空字符串)
Number              非零数值(包括无穷值)        0,NaN
Object              任意对象                    null
Undefined           N/A(不存在)                 undefined

```
>理解以上转换非常重要，因为像 if 等流控制语句会自动执行其他类型值到布尔值的转换，例如：

```
let message = "Hello world!"; 
if (message) { 
 console.log("Value is true"); 
}
```
> 因为字符串 message 会被自动转换为等价的布尔值 true。由于存在这种自动转换，理解流控制语句中使用的是什么变量就非常重要
