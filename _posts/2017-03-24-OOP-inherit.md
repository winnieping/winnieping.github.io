---
layout: post
title: '继承（inherit）'
---
Javascript如何实现继承?
<!--break-->
### 继承（inherit）

继承是什么？*继承是*指一个对象直接使用另一对象的属性和方法。也指按照法律或遵照遗嘱接受死者的财产、职务、头衔、地位等。

Javascript实现继承有原型链继承，构造继承，实例继承，组合继承，寄生继承。
##### 1.父类 
	function Animal(name){
	  this.name = name || 'Animal'; //属性
	  this.sleep = function(){
	      console.log(this.name + ' is sleeping'); 
	  } //实例方法
	}
	Animal.prototype.eat = function(food){
		console.log(this.name + ' eat ' +food );
	} //原型方法
##### 2.原型链继承
将父类实例作为子类原型,复制父类的实例属性和原型方法。

```
function Cat (){
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

// Test Code
var Animal = new Animal();
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat.eat('mouse'));
console.log(cat instanceof Animal);
console.log(cat instanceof Cat);
```

实例是子类的实例，也是父类的实例

##### 3.构造继承
使用父类的构造函数来增强子类实例，等于复制父类的实例属性。

	function Cat(name){
	  Animal.call(this);
	  this.name = name || 'Tom';
	}
	
	// Test Code
	var cat = new Cat();
	console.log(cat.name);
	console.log(cat.sleep());
	console.log(cat.eat('mouse'));
	console.log(cat instanceof Animal);
	console.log(cat instanceof Cat);
##### 4.实例继承
为父类实例添加新特性，作为子类实例返回。
	function Cat(name){
	  var instance = new Animal();
	  instance.name = name || 'name';
	  return instance;
	}
	
	// Test Code
	var cat = new Cat();
	console.log(cat.name);
	console.log(cat.sleep());
	console.log(cat.eat('mouse'));
	console.log(cat instanceof Animal);
	console.log(cat instanceof Cat);
##### 5.组合继承
构造继承和原型继承的结合，缺点调用了两次父类构造函数，生成两份实例。

	function Cat(name){
	  Animal.call(this);
	  this.name = name || 'Tom';
	}
	Cat.prototype = new Animal();
	
	// Test Code
	var cat = new Cat();
	console.log(cat.name);
	console.log(cat.sleep());
	console.log(cat.eat('mouse'));
	console.log(cat instanceof Animal);
	console.log(cat instanceof Cat);
##### 6.寄生组合继承
通过寄生方式，砍掉父类的实例属性。在调用两次父类的构造的时候，就不会初始化两次实例方法/属性。
function Cat(name){
	Animal.call(this);
	this.name = name || 'Tom';
}
(function (){
	var Super = function(){};
	Super.prototype = Animal.prototype;
	Cat.prototype = new Super();
})();
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat.eat('mouse'));
console.log(cat instanceof Animal);
console.log(cat instanceof Cat);
