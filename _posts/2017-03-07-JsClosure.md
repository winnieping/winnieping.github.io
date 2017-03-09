---
layout: post
title: 'Javascript闭包（Closure）'
---
# Javascript闭包（Closure）
在理解闭包前，先清楚js的作用域只有两种，全局作用域和方法作用域。  
全局作用域很好理解，方法作用域就是function形成一个独立作用域，而且方法作用域还能嵌套。  
先看看作用域：
```
var g = 0;
function f() {
    // 这里面就形成了一个方法作用域, 能够保护其中的变量不能被外部访问
    // 方法作用域能够访问全局作用域
    var a = 1;
    console.log(g);

    // 嵌套方法作用域
    function ff() {
        // 这里面再度形成了一个方法作用域
        // 其中可以访问外部的那个方法作用域
        var aa = 2;
        console.log(a);
    }

    // 出了 ff 的作用域就不能访问其中的东西了
    // console.log(aa); // 报错 ReferenceError: aa is not defined
}
f();
// console.log(a); // 报错 ReferenceError: a is not defined  
```
下面开始讲闭包 
```
for(var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}  

1. 首先说说为什么最终输出的是10次10, 而不是你想象中的 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
   因为setTimeout是异步的!
   你可以想象由于setTimeout是异步的, 因此我们将这个for循环拆成2个部分
   第一个部分专门处理 i 值的变化, 第二个部分专门来做setTimeout
   因此我们可以得到如下代码
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

   这样一拆后, 我相信你肯定知道之前那个for循环的运行结果了.
   由于循环中的变量 i 一直在变, 最终会变成10, 而循环每每执行setTimeout时, 其中的方法还没有真正运行, 等真正到时间执行时, i 的值已经变成 10 了!
   i 变化的整个过程是瞬间完成的, 总之比你异步要快, 就算你setTimout是0毫秒也一样, 会先于你执行完成.  

2. 那么为什么setTimeout中匿名function没有形成闭包呢?
   因为setTimeout中的匿名function没有将 i 作为参数传入来固定这个变量的值, 让其保留下来, 而是直接引用了外部作用域中的 i, 因此 i 变化时, 也影响到了匿名function.

   因此如果我们定义一个外部函数, 让 i 作为参数传入即可"闭包"我们要的变量了!!
   for (var i = 0; i < 10; i++) {
       // 注意关键是我们把想要闭包的值当参数传入一个方法
       // 这个方法 return 一个新的方法 -- 闭包!!
       setTimeout(fn(i), 1000);
   }
   function fn() { // 为了深刻理解闭包, 这个函数我没有用参数
       // 神奇的"闭包"发生在这一步, 其实就是作用域和值复制在起了关键作用,
       // 对于数字/字符等类型是复制值, 而不是引用
       var a = arguments[0];
       return function() {
           console.log(a); // 注意现在我操作的变量已经变成 a 了,
                                     // 已经和 i 没有半毛线关系了!
                                     // 而 a 的值就是当时执行时赋予的一个确定值,
                                     // 不会因 i 的变化而变化了!
       };
   }

3. 再换成更简洁的方式看你能不能真正理解闭包
   for (var i = 0; i < 10; i++) {
       (function(a) {
           // 变量 i 的值在传递到这个作用域时被复制给了 a,
           // 因此这个值就不会随外部变量而变化了
           setTimeout(function() {
               console.log(a);
           }, 1000);
       })(i); // 我们在这里传入参数来"闭包"变量
   }

这就是我所理解的闭包, 简单点说就是专门用来"包养"变量的.
```
以上参考 https://www.douban.com/note/293295975/  
### 拓展——自执行匿名函数  
var f = function(){****}; //定义一个函数  
f(); //当我们在函数后面加个括号就能实现自执行，那么是不是意味着下面这个也可以自执行?  
function(){****}();  //SyntaxError: Unexpected token   
并不能实现！  
#### 1、原因：  
在一个表达式后面加上括号()，该表达式会立即执行，但是在一个语句后面加上括号()    ，是完全不一样的意思，他的只是分组操作符。    
#### 2、解决：  
这个问题，我们只需要用大括弧将代码的代码全部括住就行了，因为JavaScript里括弧()里面不能包含语句。
解析器在解析function关键字的时候，会将相应的代码解析成function表达式，而不是function声明。  
#### 3、用法：  
用于闭包保存状态，eg见上文。利用这些被lock住的传入参数，自执行函数表达式可以有效地保存状态。
```
for (var i = 0; i < 5; i++) {
	 (function(i){
		setTimeout(function() {
		    console.log(i);
		  }, 1000 * i);
	 })(i)
}
```
### 拓展——箭头函数 
箭头函数相当于匿名函数   
### 1、单个参数  
x => x + x     
相当于 function　(x) {  
&nbsp;&nbsp;return x + x;  
&nbsp;&nbsp;}
### 2、多个参数   
(x,y) => x + y  
相当于 function　(x,y) {  
&nbsp;&nbsp;	return x + y;  
&nbsp;&nbsp;}

