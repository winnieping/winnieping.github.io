---
layout: post
title: 'ValueCopy'
---
ECMAScript变量的值类型有两种，基本类型和引用类型.
<!--break-->
###  浅拷贝与深拷贝
#### 1.我们来看一段代码: 
```
var a = new Object();
a.value = 1;
b = a;
b.value = 2;
alert(a.value);
```
输出为2，这是为什么呢。  
我们从存储来看：  
基本类型一般都是number，string，boolean，null，undefind这些，他们占据的空间是固定的，所以把它们存储在栈中，栈按照后进先出的原则存储数据。  
引用类型一般是object，function，array，它们的大小是不固定的，所以不能存储在栈区，而是分配到堆区，堆区是内存中的动态区域，程序运行的时候动态分配给代码和堆栈，虽然这些数据大小不固定，但是地址的大小是固定的，所以我们在栈区存储对象在堆区的地址即可。这个地址即指针。 

#### 2.浅拷贝
var a = [{a:1},{b:2},{c:3}]

var b = a;

b[0].a = 4;

则a[0] = {a:4} 这是因为当复制对象中存在对象或数组时，b=a只是得到一个内存地址，不是真正的复制，修改b的同时也会修改a。

#### 3.深拷贝：
```
function deepCopy(p, c) {
　　　　var c = c || {};
　　　　for (var i in p) {
　　　　　　if (typeof p[i] === 'object') {
　　　　　　　　c[i] = (p[i].constructor === Array) ? [] : {};
　　　　　　　　deepCopy(p[i], c[i]);
　　　　　　} else {
　　　　　　　　　c[i] = p[i];
　　　　　　}
　　　　}
　　　　return c;
　　}
```