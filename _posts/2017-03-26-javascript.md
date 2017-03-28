---
layout: post
title: 'JavaScript'
---
关于JavaScrip的基础知识。
<!--break-->
### JavaScrip
#### 1.定义变量的几种方法
有 var let const 
var 和 let的区别  
es6之前，我们定义用var，let的作用域比var更严格。
```
var a = [];
for(var i=0;i<10;i++) { 
    a[i] = function(){
        console.log(i);
    };
} 
a[6]();
```
```
var a = [];
for(let i=0;i<10;i++) { 
    a[i] = function(){
        console.log(i);
    };
} 
a[6]();
```
由于js不存在块级定义域，所以第一个i溢出。而使用let定义变量，会有一个块级作用域的
概念，每次调用的时候let都是新的。
const定义不可改变的常量。

#### 2.由var的作用域的思考
之前提到js作用域只有全局作用域和方法作用域，但是为什么var定义的变量能在函数外被访问到呢。
我们来看两个例子。
```
for(var i=0;i<3;i++) { 
	var k = i + 1;
} 
console.log(i,k);
```
```
function foo(){
	var a = 6;
	return a;
}
foo();
console.log(a);
```
为什么下面的a是undifind?
事实是：JavaScript以函数为界，每个函数内部拥有一个局部作用域；任何其他的块（包括普通代码块，for循环、if、while等代码块）不存在局部作用域，使用var声明的变量可以直接穿过这些代码块，可以被外部代码访问到。
#### 3.内存占用
如果在函数中使用var定义变量，在函数退出后，变量会被销毁，释放内存。


