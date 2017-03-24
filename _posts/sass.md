---
layout: post
title: 'Sass'
---
### Sass用法
#### 1.变量
$ft12:12px;
p {
	font-size:$ft12;
}
#### 2.允许选择器嵌套
div h1 {
	....
}
div{
	h1{
		....
	}
}
#### 3.css 继承
.class1 {
　　border: 1px solid #ddd;
}
.class2{
	@extend .class1;
}
#### 3.css 循环
@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}
