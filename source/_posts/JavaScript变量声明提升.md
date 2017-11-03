---
title: JavaScript变量声明提升
layout: page
tags: JavaScript
date: 2015-06-10 14:03:07
---

原文地址：[Segmentfault](https://segmentfault.com/a/1190000002582759)

### hoisting机制

javascript的变量声明具有hoisting机制，javascript引擎在执行的时候，会把所有变量的声明都提升到当前作用域的最前面。
例如：

``` bash
var v = "hello";
(function(){
console.log(v);
var v = "world";
})();
       //result:undefined
             /*上面的代码等价下面这段*/
var v = "hello";
(function(){
var v; //declaration hoisting
console.log(v);
v = "world";
})();         //result:undefined
```

**说明：function作用域里的变量v遮盖了上层作用域变量v**

改动代码为：

``` bash
var v = "hello";
if(true){
console.log(v);
var v = "world";
}      //result:hello
```
**说明：javascript是没有块级作用域的。函数是javascript中唯一拥有自身作用域的结构。**

<!-- more -->

### 声明，定义与初始化

声明宣称一个名字的存在，定义则为这个名字分配存储空间，而初始化则是为名字分配的存储空间赋值，用C++来表达这三个概念

``` bash
extern int i;//这是声明，表明名字i在某处已经存在了
int i;//这是声明并定义名字i,为i分配存储空间
i = 0;//这是初始化名字i,为其赋初值为0
```

而在javascript中则是这样

``` bash
var v;//声明变量v;
v="hello"; //(定义并)初始化变量v
```

*在函数中使用var关键字进行显式申明的变量是做为局部变量，而没有用var关键字，使用直接赋值方式声明的是全局变量。当我们使用访问一个没有声明的变量时，JS会报错。而当我们给一个没有声明的变量赋值时，JS不会报错，相反它会认为我们是要隐式申明一个全局变量。*

### 声明提升

当前作用域内的声明都会提升到作用域的最前面，包括变量和函数的声明

``` bash
(function(){
var a = "1";
var f = function(){};
var b = "2";
var c = "3";
})(); 
```
变量a,f,b,c的声明会被提升到函数作用域的最前面，类似如下：

``` bash
(function(){
var a,f,b,c;
a = "1";
f = function(){};
b = "2";
c = "3";
})(); 
```
又比如：

``` bash
(function(){
//var f1,function f2(){}; //hoisting,被隐式提升的声明

f1(); //ReferenceError: f1 is not defined
f2();

var f1 = function(){};
function f2(){}
})(); 
```
**说明：函数表达式并没有被提升，这也是函数表达式与函数声明的区别。**

### 命名函数表达式

可以像函数声明一样为函数表达式指定一个名字，但这并不会使函数表达式成为函数声明。命名函数表达式的名字不会进入名字空间，也不会被提升。

``` bash
var f = function foo(){
return typeof foo; // foo是在内部作用域内有效
};
// foo在外部用于是不可见的
typeof foo; // "undefined"
f(); // "function"
```

**命名函数表达式的名字只在该函数的作用域内部有效。，因为规范规定了标示符不能在外围的作用域内有效**
