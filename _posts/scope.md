---
layout: post
title: 'JS变量,函数提升与作用域'
---
### JS变量提升与函数提升
#### 1.引入
```
var bar=1;
function test(){
  console.log(bar);     //undeifned
  var bar=2; 
  console.log(bar);  //2
}
test();
```
这就是JavaScript的变量提升，虽然变量bar的定义在后面，不过浏览器在解析的时候，
会把变量的定义放到最前面，相当于下面：
```
var bar=1;
function test(){
  var bar;
  console.log(bar);   //undefined
  bar=2; 
  console.log(bar);   //2
}
```
请注意变量赋值并没有被提升，只是声明被提升了。

#### 2.函数提升
```
function test() {  
    foo(); // TypeError "foo is not a function"  
    bar(); // "this will run!"  
    var foo = function () { // 变量指向函数表达式  
        alert("this won't run!");  
    }  
    function bar() { // 函数声明 函数名为bar  
        alert("this will run!");  
    }  
}  
test();
```
函数提升和变量提升不同，函数体也会一同被提升。但是请注意，函数的声明有两种方式，只有函数式的声明才会连同函数体一起被提升。foo的声明会被提升，但是它指向的函数体只会在执行的时候才被赋值。
#### 3. 看下面的例子
```
var a = 1;  
function b() {  
    a = 10;  
    return;  
    function a() {}  
}  
b();  
alert(a); 
```
相当于：
```
var a = 1;  
function b() { 
	function a() {}  //相当于这里定义一个局部变量a
    a = 10;  
    return; 
    alert(a);     
}  
b();  
alert(a); 
```
现在可以看到局部变量的a是10，外部的是1；

#### 2.作用域
```
var x = 1;  
if (x) {  
    var x = 2;  
    console.log(x); // 2  
}  
console.log(x); // 2
```
因为javascript是函数作用域。这是和c家族语言最大的不同。该程序里面的if并不会创建新的作用域
为了实现本来的结果，我们使用闭包来解决。
```
var x = 1;  
if (x) {  
    (function () {  
        var x = 2;
        console.log(x); // 2    
    }());  
}
console.log(x); // 1
```

