# 1.JS语言基础
* ECMAScript 中一切都区分大小写
* 标识符命名规则
> 标识符，就是变量、函数、属性或函数参数的名称

```
1. 第一个字符必须是一个字母、下划线（_）或美元符号（$）；
2. 剩下的其他字符可以是字母、下划线、美元符号或数字。
3. 按照惯例，ECMAScript 标识符使用驼峰大小写形式，即第一个单词的首字母小写，后面每个单词
的首字母大写
4. 注意 关键字、保留字、true、false 和 null 不能作为标识符
```
# 2. 变量
> ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据;  

* 有 3 个关键字可以声明变量：var、const 和 let 【注释：const 和 let 只能在 ECMAScript 6 及更晚的版本中使用】

##  2.1 var
###  定义未初始化的情况下，变量会保存一个特殊值 undefined

```
var message;
consolo.log(message)// undefined
```

### 2.1.1. var 声明作用域
> 第一种：使用 var 操作符定义的变量会成为包含它的函数的局部变量

```
function test() { 
 var message = "hi"; // 局部变量
} 
test(); 
console.log(message); // 出错！
```
> message 变量是在函数内部使用 var 定义的。函数叫 test()，调用它会创建这个变量并给它赋值。调用之后变量随即被销毁，因此示例中的最后一行会导致错误

> 第二种：在函数内定义变量时省略 var 操作符，可以创建一个全局变量：

```
function test() { 
 message = "hi"; // 全局变量
} 
test(); 
console.log(message); // "hi" 
```

> 去掉之前的 var 操作符之后，message 就变成了全局变量。只要调用一次函数 test()，就会定义这个变量，并且可以在函数外部访问到。

> 注意

* 虽然可以通过省略 var 操作符定义全局变量，但不推荐这么做。
* 在局部作用域中定义的全局变量很难维护，也会造成困惑。这是因为不能一下子断定省略 var 是不是有意而为之。
* 在严格模式下，如果像这样给未声明的变量赋值，则会导致抛ReferenceError。
* 在严格模式下，不能定义名为 eval 和 arguments 的变量，否则会导致语法错误。

### 2.1.2. var 声明提升
> 这是因为使用这个关键字声明的变量会自动提升到函数作用域顶部：

```
function foo() {
 console.log(age); 
 var age = 26; 
} 
foo(); // undefined 
```
> 之所以不会报错，是因为 ECMAScript 运行时把它看成等价于如下代码：

```
function foo() { 
 var age; 
 console.log(age); 
 age = 26; 
} 
foo(); // undefined 
```
>这就是所谓的“提升”（hoist），也就是把所有变量声明都拉到函数作用域的顶部

```
function foo() {
 var age = 16; 
 var age = 26; 
 var age = 36;   
 console.log(age); 
} 
foo(); // 36
// 等价于
function foo() {
 var age; 
 age = 16;
 age = 26; 
 age = 36;   
 console.log(age); 
} 
foo(); // 36
```
## 2.2 let 声明
> let与var的区别？

1. let 声明的范围是块作用域，而 var 声明的范围是函数作用域。

```
if (true) { 
 var name = 'Matt'; 
 console.log(name); // Matt 
} 
console.log(name); // Matt
```
```
if (true) { 
 let age = 26; 
 console.log(age); // 26 
} 
console.log(age); // ReferenceError: age 没有定义
```
> 在这里，age 变量之所以不能在 if 块外部被引用，是因为它的作用域仅限于该块内部。【块作用域是函数作用域的子集，因此适用于 var 的作用域限制同样也适用于 let。】

2. let 也不允许同一个块作用域中出现冗余声明
```
var name; 
var name; 
let age; 
let age; // SyntaxError；标识符 age 已经声明过了

```
```
对声明冗余报错不会因混用 let 和 var 而受影响。这两个关键字声明的并不是不同类型的变量，
它们只是指出变量在相关作用域如何存在。
var name; 
let name; // SyntaxError 
let age; 
var age; // SyntaxError
```











# 1.JS原始数据类型有哪些？引用数据类型有哪些？
* 在 JS 中，存在着 6 种原始值，分别是：

```
boolean
null
undefined
number
string
symbol
```
* 引用数据类型: 对象Object（包含普通对象-Object，数组对象-Array，正则对象-RegExp，日期对象-Date，数学函数-Math，函数对象-Function）

# 2.typeof 是否能正确判断类型？
* 对于原始类型来说，除了 null 都可以调用typeof显示正确的类型。
```
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
```
* 但对于引用数据类型，除了函数之外，都会显示"object"。

```
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```
