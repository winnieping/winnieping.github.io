---
layout: post
title: 'Javascript闭包（Closure）'
---
js的作用域只有两种，全局作用域和方法作用域，不存在块级作用域。
<!--break-->
## 闭包
![closures.jpg](http://localhost:4000/assets/closures.jpg);
### 引言  
>在理解闭包前，要先了解作用域。
>js的作用域只有两种，全局作用域和方法作用域。        
>全局作用域很好理解，
>方法作用域就是function形成一个独立作用域，方法作用域还能嵌套。

先看看作用域：
```
var g = 0;
function f() {
    var a = 1;
    console.log(g);
    // 这里面就形成了一个方法作用域, 能够保护其中的变量不能被外部访问,方法作用域能够访问全局作用域

    function ff() {
        var aa = 2;
        console.log(aa);
        // 嵌套方法作用域,这里面再度形成了一个方法作用域,其中可以访问外部的那个方法作用域
    }
    console.log(aa); // 报错: aa is not defined,出了 ff 的作用域就不能访问其中的东西了
}
f();
console.log(a); // 报错: a is not defined  
```
###闭包形成的原因

简单的说，Javascript允许使用内部函数

**即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。**

JS的垃圾回收机制并不会回收这一部分资源，因而该部分的变量被保存下来。

### 闭包的概念 

>官方：可以包含自由（未绑定到特定对象）变量的代码块，变量是指在定义代码块的环境中定义的，而非上下文或全局。

实际上可以理解闭包就是写一个函数，在函数內默认新增一个保存传入值的变量，该变量不随其他定义改变。同时外部变量也不会因为函数內的变量值而改变。

###### 一、我们来看一个闭包经典案例

```
for(var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}  
```
1. 代码拆分
  // 第一个部分
     i++;
     ... 
     i++; // 总共做10次  
  // 第二个部分
     setTimeout(function() {
        console.log(i);
     }, 1000);
     ...
     setTimeout(function() {
        console.log(i);
     }, 1000); // 总共做10次
  因为setTimeout是异步的，先执行的函数是i++，i 一直在变, 最终会变成10, 但是循环在执行setTimeout时, 其中的方法还没有真正运行, 等真正到时间执行时, i 的值已经变成 10 。
  所以最终输出的是10次10, 而不是想象中的 0, 1, 2, 3, 4, 5, 6, 7, 8, 9。

2.闭包解决。
我们把i作为参数传入function，让其保留下来, 而是直接引用了外部作用域中的 i。
```
   for (var i = 0; i < 10; i++) {
       setTimeout(fn(i), 1000);
   }
   function fn() { 
       var a = arguments[0];
       return function() {
           console.log(a); 
       };
   }
```
//第一个函数我们把想要闭包的值当参数传入一个方法
//第二个函数我们让a = arguments[0]，数字/字符类型的是复制值， 
//而不是引用，return里面操作的变量已经变成a，和i没有关系了，不会因 i 的变化而变化。

3.换成简洁的方式

```
 for (var i = 0; i < 10; i++) {
       (function(a) {
           setTimeout(function() {
               console.log(a);
           }, 1000);
       })(i); 
   }
```

// 变量 i 的值在传递给functio时被复制给了a, 因此这个值就不会随外部变量而变化了。  

4.第三种解决方式

```
function outPut(i){
  setTimeout(function() {
        console.log(i);
    }, 1000);
}
for(var i = 0; i < 10; i++) {
    outPut(i)
}
```

5.第四种解决方式

采用let代替var，因为let具有独立作用域，在这个例子中，相当于每次循环都会把i重新声明一次并初始化一次。

```
for(let i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}
```

##### 二、另一个案例

```
var x = 10;
function f1 (){
    x = 1;
    return function f2(){
        x++;
        console.log(x);
    }
}
var c= f1();
c();
console.log(x);
```



### 闭包的this
this对象是运行时基于函数的执行环境绑定的。
在全局函数中，this等于window，而当函数被当作某个对象的方法调用时，this等于那个对象。
匿名函数的执行环境具有全局性，因此其this对象通常指向window。
我们来看两道的思考题：
```
var name = "The Window";
var object = {
    name : "My Object",
    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};
alert(object.getNameFunc()());
/*输出	the window */
```
```
var name = "The Window";
var object = {
    name : "My Object",
    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
};
alert(object.getNameFunc()());
/*输出  my Object	*/
```
对于最后返回的这个匿名函数，它是作为一个独立的函数返回的，它的调用域是在全局上，所以会输出全局变量name。
当加上var that = this后，因为getNameFunc是object内部的函数，所以它调用的上下文this保存的是object的信息，把它保存到that变量，这样作为内部函数的匿名函数就可以直接访问object的name了。

### 闭包的内存模型
我们仍是以这个函数作为分析：
```
   for (var i = 0; i < 10; i++) {
       setTimeout(fn(i), 1000);
   }
   function fn() {   
       var a = arguments[0];
       return function() {
           console.log(a); 
       };
   }
   
  用函数将i的值拷贝赋给a，function中获得a的值并返回
```
作用域链内存模型如下图：
JavaScript函数调用时侯，会创建一个执行环境，为每一个函数增加一个属性SCOPE（作用域链）,
这个属性来指向一块内存，这块内存中包含有所有上下文的变量。
变量的顺序始终是当前执行的代码所在的环境的变量对象在最前端。
<img src="/../assets/closure02.png">
 setTimeout作用域链的最高位指向全局作用域，全局作用域有一些this，window属性。作用域链的低位指向自己的作用域，有fn function一个方法。

匿名函数也有它的作用域链。它的高位指向全局作用域，中间位指向包含它的fn函数的作用域，低位才是指向自己的作用域

Javascript内存回收机制：
如果一个对象不再被引用，那么这个对象就会被GC回收。如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。

当执行完setTimeout，i=0时，内存回收机制开始回收，闭包所在作用域不会被回收。

会发现匿名函数有指向settimeout的作用域，但是settimeout并没有引用匿名函数。
此时就不会回收这块作用域内存。
而fn的作用域链和函数本身的内存会作为垃圾被回收掉。

### 闭包的缺点
对于一般的函数而言，其执行结束之后就会释放局部变量所占内存，但是闭包不会。

所以当闭包作用域链中保存的引用变量不需要的时候，应设置为null。
``` 
function assignHandler(){  
    var element = $('id');  
    var id = elment.id;  
  
    element.onclick = function(){  
        alert(id);  
    };  
    element = null;  
}
```
#### 闭包的实际应用

既然闭包的作用是抛出内部变量给外部函数调用和让变量不受上下文定义的影响，那么我们来看看它的应用。

```
第一种：比如jQuery的$对象，就是用闭包实现的返回对象提供给外部引用。
第二种：当函数是异步执行函数的时候，可以用闭包来保存变量防止因为外部定义导致值变化。
```



### 总结

闭包最大的作用就是持久保留住局部变量，通过调用嵌套匿名函数可以把闭包内部作用域中的变量值存储在内存中而不在函数调用(实际调用的为嵌套匿名函数，不是外围函数)完毕后就销毁。当然使用不当会造成内存泄漏等问题，所以使用谨慎使用。



### 思考题

```
var d= 4;
function e(){
	var d= 1;
	return function(){
		console.log("d",this.d++,d++);
	}
}
var c = e();
c();
console.log(d)
```



