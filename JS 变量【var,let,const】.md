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

### 2.2.1. let 声明的范围是块作用域，而 var 声明的范围是函数作用域。

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

### 2.2.2. let 也不允许同一个块作用域中出现冗余声明
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
### 2.2.3. 暂时性死区:let 声明的变量不会在作用域中被提升

```
// name 会被提升
console.log(name); // undefined 
var name = 'Matt'; 
// age 不会被提升
console.log(age); // ReferenceError：age 没有定义
let age = 26; 

```
> 在 let 声明之前的执行瞬间被称为“暂时性死区”（temporal dead zone），在此
阶段引用任何后面才声明的变量都会抛出 ReferenceError。

### 2.2.4. 全局声明
> 与 var 关键字不同，使用 let 在全局作用域中声明的变量不会成为 window 对象的属性（var 声明的变量则会）。

```
var name = 'Matt'; 
console.log(window.name); // 'Matt' 
let age = 26; 
console.log(window.age); // undefined

```
### 2.2.5. 条件声明
> 在使用 var 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合
并为一个声明。因为 let 的作用域是块，所以不可能检查前面是否已经使用 let 声明过同名变量，同
时也就不可能在没有声明的情况下声明它。

```
<script>
    var name = 'Nicholas';
    let age = 26; 
</script>
<script>
    // 假设脚本不确定页面中是否已经声明了同名变量
    // 那它可以假设还没有声明过
    var name = 'Matt';
    // 这里没问题，因为可以被作为一个提升声明来处理
    // 不需要检查之前是否声明过同名变量
    let age = 36;
    // 如果 age 之前声明过，这里会报错
</script>
```
### 2.2.6.  for 循环中的 let 声明
> 在 let 出现之前，for 循环定义的迭代变量会渗透到循环体外部：

```
for (var i = 0; i < 5; ++i) { 
 // 循环逻辑 
} 
console.log(i); // 5 
```
> 改成使用 let 之后，这个问题就消失了，因为迭代变量的作用域仅限于 for 循环块内部：

```
for (let i = 0; i < 5; ++i) { 
 // 循环逻辑
} 
console.log(i); // ReferenceError: i 没有定义

```
> 在使用 var 的时候，最常见的问题就是对迭代变量的奇特声明和修改：

```
for (var i = 0; i < 5; ++i) { 
 setTimeout(() => console.log(i), 0) 
} 
// 你可能以为会输出 0、1、2、3、4 
// 实际上会输出 5、5、5、5、5 
```
> 之所以会这样，是因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在之后执行超时逻辑时，所有的 i 都是同一个变量，因而输出的都是同一个最终值。
而在使用 let 声明迭代变量时，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量。

> 每个 setTimeout 引用的都是不同的变量实例，所以 console.log 输出的是我们期望的值，也就是循
环执行过程中每个迭代变量的值。

```
for (let i = 0; i < 5; ++i) { 
 setTimeout(() => console.log(i), 0) 
} 
// 会输出 0、1、2、3、4 
```

> 这种每次迭代声明一个独立变量实例的行为适用于所有风格的 for 循环，包括 for-in 和 for-of
循环。

## 2.3 const 声明
> const 的行为与 let 基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且尝试修改 const 声明的变量会导致运行时错误。
1. const 也不允许重复声明
2. const 声明的作用域也是块
3. const 声明变量时必须同时初始化变量
4. const 声明的限制只适用于它指向的变量的引用。换句话说，如果 const 变量引的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制

```
const person = {}; 
person.name = 'Matt'; // ok
```







